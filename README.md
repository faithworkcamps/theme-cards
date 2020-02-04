# @faithworkcamps/theme-cards

A modified version of [Thumbsup's](https://github.com/thumbsup/thumbsup) ["Card Theme"](https://github.com/thumbsup/theme-cards) styled to match [faithworkcamps.com](https://github.com/faithworkcamps/faithworkcamps.github.io)

---

## config.json

```
{
  "input": "/input",
  "output": "/output",
  "title": "Faith Workcamps:",
  "albums-output-folder": "./albums",
  "sort-albums-by": "title",
  "sort-albums-direction": "desc",
  "sort-media-by": "filename",
  "photo-download": "copy",
  "video-download": "copy",
  "cleanup": true,
  "theme-path": "/config/theme-cards",
  "usage-stats": false,
  "homeAlbumName": "Albums"
}
```

## Docker

Testing the theme

```
docker run -t  \
  -v /mnt/user/appdata/thumbsup:/config  \
  -v /mnt/user/nextcloud/faithworkcamps/files/Documents/Workcamp/Workcamp_Website_Photos:/input:ro  \
  -v /mnt/user/backup/faithworkcamps.com/gallery:/output  \
  -e $(id -u):$(id -g)  \
  -v /etc/localtime:/etc/localtime  \
  thumbsupgallery/thumbsup  \
  thumbsup --config /config/config.json
```

Publishing the theme to AWS S3:

```
docker run --rm  \
  -v /mnt/user/backup/faithworkcamps.com/gallery:/thumbsup  \
  -v /mnt/user/appdata/awscli:/root/.aws  \
  xueshanf/awscli:latest  \
  aws s3 sync /thumbsup s3://${BUCKET} --delete
```

## Hosting
Set the AWS S3 bucket Properties to enable Static website hosting.
Permissions Bucket Policy when using Cloudflare to direct a custom domain

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::BUCKET/*",
            "Condition": {
                "IpAddress": {
                    "aws:SourceIp": [
                        "2400:cb00::/32",
                        "2405:8100::/32",
                        "2405:b500::/32",
                        "2606:4700::/32",
                        "2803:f800::/32",
                        "2c0f:f248::/32",
                        "2a06:98c0::/29",
                        "103.21.244.0/22",
                        "103.22.200.0/22",
                        "103.31.4.0/22",
                        "104.16.0.0/12",
                        "108.162.192.0/18",
                        "131.0.72.0/22",
                        "141.101.64.0/18",
                        "162.158.0.0/15",
                        "172.64.0.0/13",
                        "173.245.48.0/20",
                        "188.114.96.0/20",
                        "190.93.240.0/20",
                        "197.234.240.0/22",
                        "198.41.128.0/17"
                    ]
                }
            }
        }
    ]
}
```
