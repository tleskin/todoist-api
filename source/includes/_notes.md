# Notes

> A note in Todoist is a JSON object. Typically a note object will look like:

```shell
{
  "is_deleted": 0,
  "is_archived": 0,
  "content": "Note",
  "file_attachment": {
    "file_type": "text/plain",
    "file_name": "File1.txt",
    "file_size": 1234,
    "file_url": "https://example.com/File1.txt",
    "upload_state": "completed"
  }
  "posted_uid": 1855589,
  "item_id": 33548400,
  "uids_to_notify": null,
  "id": 1234,
  "posted": "Wed 01 Oct 2014 14:54:55 +0000"
}
```

```python
{
  'is_deleted': 0,
  'is_archived': 0,
  'content': 'Note',
  'item_id': 33548400,
  'posted_uid': 1855589,
  'posted': 'Wed 01 Oct 2014 14:54:55 +0000',
  'id': 1234,
  'file_attachment': {
    'file_type': 'text/plain',
    'file_name': 'File1.txt',
    'file_size': 1234,
    'file_url': 'https://example.com/File1.txt',
    'upload_state': 'completed'
  }
  'uids_to_notify': None
}
```

### Properties

Property | Description
-------- | -----------
id | The id of the note.
content | The content of the note.
item_id | The item which the note is part of.
file_attachment | A file attached to the note.

### File attachments

A file attachment is represented as a JSON object. The file attachment may point to a document, previously uploaded by the `uploadFile` API call, or by any external resource.

### Base file properties

Attribute | Description
--------- | -----------
file_name | The name of the file.
file_size | The size of the file in bytes.
file_type | MIME type.
file_url | The URL where the file is located. Note that we don't cache the remote content on our servers and stream or expose files directly from third party resources. In particular this means that you should avoid providing links to non-encrypted (plain HTTP) respources, as exposing this files in Todoist may issue a browser warning.
upload_state | Upload completion state.

### Image file properties

If you upload an image, you may provide thumbnail paths to ensure Todoist handles them appropriately. Valid thumbnail information is a JSON array with URL, width in pixels, height in pixels. Ex.: ["http://example.com/img.jpg",400,300]. "Canonical" thumbnails (ones we create by `uploadFile` API call) have following sizes: 96x96, 288x288, 528x528.

Attribute | Description
--------- | -----------
tn_l | Large thumbnail.
tn_m | Medium thumbnail.
tn_s | Small thumbnail.

### Audio file properties

If you upload an audio file, you may provide an extra attribute `file_duration` (duration of the audio file in seconds, which takes an integer value). In the web interface the file is rendered back with a `<audio>` tag, so you should make sure it's supported in current web browsers. See [supported media formats](https://developer.mozilla.org/en-US/docs/HTML/Supported_media_formats) for the reference.


## Add a note

> An example of adding a note:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "note_add", "temp_id": "$1412325057551", "timestamp": 1412325057551, "args": {"item_id": 33548400, "content": "Note1"}}]'
{"$1412325057551": 1234}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> note = api.note_add(33548400, 'Note1')
>>> api.commit()
```

Add a note.

### Required arguments

Argument | Description
-------- | -----------
item_id | The id of the item
content | The content of the note.

### Optional arguments

Argument | Description
-------- | -----------
file_attachment | A file attached to the note.

## Update a note

> An example of updating a note:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "note_update", "timestamp": 1412325418683, "args": {"id": 1234, "content": "UpdatedNote1"}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> note api.note_get_by_id(1234)
>>> note.update(content='UpdatedNote1')
>>> api.commit()
```

Update a note.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the note.

### Optional arguments

Argument | Description
-------- | -----------
content | The content of the note.
file_attachment | A file attached to the note.

## Delete a note

> An example of deleting a note:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "note_delete", "timestamp": 1412325418683, "args": {"id": 1234}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> note = api.note_get_by_id(1234)
>>> note.delete()
>>> api.commit()
```

Delete a note.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the note.