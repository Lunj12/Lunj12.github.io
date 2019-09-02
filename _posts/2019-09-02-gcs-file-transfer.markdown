---
layout: post
title:  "Google Cloud Storage File Transfer"
date:   2019-09-02 16:02:16 -0700
categories: blog posts
---

When using Google Cloud clusters for computation, we often need to transfer files between compute engines and Google Cloud Storage (GCS) buckets.

In Python, the official API `google.cloud.storage` gives a solution by using `client`, `bucket` and `blob`, because each single file in GCS is considered as a blob. The official modules to transfer files between machines and GCS are less straightforward and convenient, especially when transfering an entire directory.

In this post, Python functions are provided for easily tranfering files between local unix-based machines and GCS.

### Category
```
* Transfer a single file
* Transfer an entire folder
```

## Transfer a single file

```python
from google.cloud import storage

def upload_file_to_gcs(local_file_path, gcs_bucket_name, gcs_save_path):
    """
        upload a single file to gcs bucket
        e.g. upload file ~/Desktop/my_file.txt to gs://my_gcs_bucket/gcs_save_dir/my_file.txt
    :param local_file_path: e.g. ~/Desktop/my_file.txt
    :param gcs_bucket_name: e.g. my_gcs_bucket
    :param gcs_save_path: e.g. gcs_save_dir/my_file.txt
    :return: None
    """
    storage_client = storage.Client()
    bucket = storage_client.get_bucket(gcs_bucket_name)
    blob = bucket.blob(gcs_save_path)
    blob.upload_from_filename(local_file_path)
    print('\t Uploaded ' + local_file_path + ' to ' + 'gs://' + gcs_bucket_name + '/' + gcs_save_path)


def download_file_from_gcs(local_file_path, gcs_bucket_name, gcs_save_path):
    """
        download a single file from gcs bucket
        e.g. download file gs://my_gcs_bucket/gcs_save_dir/my_file.txt to ~/Desktop/my_file.txt
    :param local_file_path: e.g. ~/Desktop/my_file.txt
    :param gcs_bucket_name: e.g. my_gcs_bucket
    :param gcs_save_path: e.g. gcs_save_dir/my_file.txt
    :return: None
    """
    storage_client = storage.Client()
    bucket = storage_client.get_bucket(gcs_bucket_name)
    blob = bucket.blob(gcs_save_path)
    blob.download_to_filename(local_file_path)
    print('\t Downloaded ' + local_file_path + ' from ' + 'gs://' + gcs_bucket_name + '/' + gcs_save_path)
```

## Transfer an entire folder

Implementation of transferring an entire folder requires recursive strategy.

```python
from google.cloud import storage

def upload_folder_to_gcs(local_folder_path, gcs_bucket_name, gcs_folder_path, verbose=True):
	 """
        upload an entire folder to gcs bucket
        e.g. upload directory ~/Desktop/my_folder/ to gs://my_gcs_bucket/gcs_save_dir/my_folder
    :param local_folder_path: e.g. ~/Desktop/my_folder/
    :param gcs_bucket_name: e.g. my_gcs_bucket
    :param gcs_folder_path: gcs_save_dir/my_folder
    :param verbose: True or False. Whether to print out completion message for each file
    :return: None
    """
    # create storage client
    storage_client = storage.Client()
    bucket = storage_client.get_bucket(gcs_bucket_name)

    # separate files and sub folders in this folder
    files = []
    sub_folders = []
    for f_name in os.listdir(local_folder_path):
        f_path = os.path.join(local_folder_path, f_name)
        if os.path.isfile(f_path):
            files.append(f_name)
        elif os.path.isdir(f_path):
            sub_folders.append(f_name)

    # for each file, upload it to gcs
    for file_name in files:
        local_file_path = os.path.join(local_folder_path, file_name)
        gcs_file_path = os.path.join(gcs_folder_path, file_name)
        blob = bucket.blob(gcs_file_path)
        blob.upload_from_filename(local_file_path)
        if verbose:
            print('\t Uploaded ' + local_file_path + ' to ' + 'gs://' + gcs_bucket_name + '/' + gcs_file_path)

    # for each sub folder, call function upload_folder_to_gcs() recursively
    for sub_folder_name in sub_folders:
        local_sub_folder_path = os.path.join(local_folder_path, sub_folder_name)
        gcs_sub_folder_path = os.path.join(gcs_folder_path, sub_folder_name)
        upload_folder_to_gcs(local_sub_folder_path, gcs_bucket_name, gcs_sub_folder_path, verbose)


def download_folder_from_gcs(local_folder_path, gcs_bucket_name, gcs_folder_path, verbose=True):
    """
        download an entire folder to gcs bucket
        e.g. download directory gs://my_gcs_bucket/gcs_save_dir/my_folder to ~/Desktop/my_folder/
    :param local_folder_path: e.g. ~/Desktop/my_folder/
    :param gcs_bucket_name: e.g. my_gcs_bucket
    :param gcs_folder_path: gcs_save_dir/my_folder
    :param verbose: True or False. Whether to print out completion message for each file
    :return: None
    """
    gcs_folder_path = trim_gcs_save_path(gcs_bucket_name, gcs_folder_path)
    root_path_length = len(gcs_folder_path.split('/'))

    # create storage client
    storage_client = storage.Client()
    bucket = storage_client.get_bucket(gcs_bucket_name)

    # download list of files with prefix being the path of the gcs folder
    blobs = bucket.list_blobs(prefix=gcs_folder_path)
    for blob in blobs:
        if blob.name[-1] != '/':  # blob is a file
            path_list = trim_gcs_save_path(gcs_bucket_name, blob.name).split('/')
            filename = path_list.pop()

            # path to the enclosing folder;
            # absolute folder path prefix should be removed, only join the part after root path.
            local_folder_path = os.path.join(local_folder_path, *path_list[root_path_length:])

            # make create local folders to the enclosing folder, if not exist
            os.makedirs(local_folder_path, exist_ok=True)

            local_file_path = os.path.join(local_folder_path, filename)
            blob.download_to_filename(local_file_path)
            if verbose:
                print('\t Downloaded ' + local_file_path + ' from ' + 'gs://' + gcs_bucket_name + '/' + blob.name)
```