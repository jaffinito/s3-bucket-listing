Creates nice, Apache-styled, directory listings for s3 buckets using only javascript and HTML in a single index.html file.

## How it works
The script in the html downloads your XML bucket listing, parses it and simulates a webserver's text-based directory browsing mode, in this case styled after Apache's.

## Changes from the original version
Several options were removed to simplfy the script since the goal was to simulate Apache's style.  

The URL navigation to be in this form:
- _`http://data.openspending.org/worldbank/cameroon/`_

## Usage

Replace `var BUCKET_URL = 'http://???.s3.amazonaws.com';` in the HTML file with your bucket URL.  See below for details on which URL to use.

Put the html file in the bucket root and set both the index and error values to index.html.

#### BUCKET_URL variable
Valid options = `''` (default) or your _bucket URL_, e.g.

`http://BUCKET.s3-REGION.amazonaws.com` (both http & https are valid)

Note that US east-1 region is **different** in that the S3 bucket endpoint does not include a location spec but the website version does:

* Website endpoint: _`http://example-bucket.s3-website-us-east-1.amazonaws.com/`_
* S3 bucket endpoint (for RESTful calls and BUCKET_URL): _`http://example-bucket.s3.amazonaws.com/`_

- Do __NOT__ put a trailing '/', e.g. `http://BUCKET.s3-REGION.amazonaws.com/`
- Do __NOT__ put S3 website URL, e.g. `http://BUCKET.s3-website-REGION.amazonaws.com`

This variable tells the script where your bucket XML listing is, and where the files are.
If the variable is left empty, the script will use the same hostname as the _index.html_.

#### S3B_ROOT_DIR variable
**This is untested with the modifications**
Valid options = `''` (default) or `'SUBDIR_L1/'` or `'SUBDIR_L1/SUBDIR_L2/'` or etc.

- Do __NOT__ put a leading '/',     e.g. `'/SUBDIR_L1/'`
- Do __NOT__ omit the trailing '/', e.g. `'SUBDIR_L1'`

This will disallow navigation shallower than your set directory.

Note that this only disallows navigation to shallower directories, but __NOT__ access. Any person with knowledge of the existence of bucket XML listings will be able to manually access those files.

Use Amazon S3 permissions to set granular file permissions.

## S3 website bucket permissions

You must setup the S3 website bucket to allow public read access. 

* Grant `Everyone` the `List` and `View` permissions:
![List & View permissions](https://f.cloud.github.com/assets/227505/2409362/46c90dbe-aaad-11e3-9dee-10e967763770.png) 
 * Alternatively you can assign the following bucket policy if policies are your thing:

```
{
    "Version": "2008-10-17",
    "Statement": [
        {
            "Sid": "AllowPublicRead",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::{your-bucket-name}/*"
        }
    ]
}
```
* Assign the following CORS policy
```
<CORSConfiguration>
 <CORSRule>
   <AllowedOrigin>*</AllowedOrigin>
   <AllowedMethod>GET</AllowedMethod>
   <AllowedHeader>*</AllowedHeader>
 </CORSRule>
</CORSConfiguration>
```

##Original Copyright and License

Original version from: https://github.com/rgrp/s3-bucket-listing
Copyright 2012-2013 Rufus Pollock.

Licensed under the MIT license:

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

