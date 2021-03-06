# exr_headers
pure python .exr header reader. reads all metadata and header data from the exr frame directly. Including Timecode!

Because we are parsing out the binary data directly im sure there are edge cases where this doesn't work and has only really been tested on basic metadata.
To this end it will also include what type we think the data should be. So if you get something back that looks wonky (ie timecode) it might just mean we need to do a bit more manipulation to read it in a sensible way.

If you use this project for anything please let me know, pull requests are super welcome!


# Basic Usage
```
import exr_header as exh


def read_exr(f):
    with open(f, 'rb') as fd:
        exr = exh.ExrHeader()
        if exr.read(fd):
            print(exr.attributes())
        else:
            print("unknown file or error")

    return exr
    
exr = read_exr('/path/to/a/frame.exr')
print(exr.get()['nuke/version'])
print(exr.getValue("nuke/version")

```
### example output
```
['version', 'nuke/node_hash', 'compression', 'pixelAspectRatio', 'displayWindow', 'channels', 'nuke/full_layer_names', 'dataWindow', 'screenWindowCenter', 'nuke/version', 'type', 'screenWindowWidth', 'lineOrder']
{'type': 'string', 'value': '10.0v4'}
'10.0v4'
```

# Reading Timecode
timecode is stored in hex so to read it you need to do some more manipulation...
_TODO:TC example_

its something like...
```
t=str(hex(exr.getValue("timecode")[0]))[2:]
t=':'.join([t[0:1],t[2:3],t[3:4],t[5:6]])
```

# Api

ExrHeader functions
### read(file)
  takes a file handle and reads the binary header of all the metadata

### attributes()
  returns a list of all availible keys in the header
  
### get(key) 
  returns the dictionary of the type and value of the key from the header
  
### getValue(key)
  returns just the value of the key from the header
