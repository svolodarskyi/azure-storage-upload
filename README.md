# azure-storage-upload
upload files to a Azure Storage

To upload files to Azure Storage install the library via terminal:

`pip install azure storage blob`


```python
import os
from azure.storage.blob import ContainerClient

dir_files = os.path.join(os.path.dirname(os.path.abspath(__file__)), "files\\") 

config = {
            "storage_connection_string": "connection_string",
            "inflight_container_name": "inflight",
            "file_folder": dir_files
        }

def get_files(dir):
    with os.scandir(dir) as contents:
        for item in contents:
            if item.is_file() and not item.name.startswith('.'): #hiddenfiles
                yield item

def upload(files, connection_string, container_name):
    container_client = ContainerClient.from_connection_string(connection_string, container_name)
    
    for file in files:
        blob_client = container_client.get_blob_client(file.name)
        with open(file.path, "rb") as data:
            blob_client.upload_blob(data)
            print(f"{file.name} uploaded")

files_upload = get_files(config["file_folder"])

upload(files_upload, config["storage_connection_string"], config["inflight_container_name"])

```
