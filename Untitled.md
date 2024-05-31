{

    "Version": "2012-10-17",

    "Statement": [

        {

            "Sid": "AllowSSLRequestsOnly",

            "Effect": "Deny",

            "Principal": "*",

            "Action": "s3:*",

            "Resource": [

                "arn:aws:s3:::dev-sdp-web-components",

                "arn:aws:s3:::dev-sdp-web-components/*"

            ],

            "Condition": {

                "Bool": {

                    "aws:SecureTransport": "false"

                }

            }

        },

        {

            "Sid": "AllowFromStg",

            "Effect": "Allow",

            "Principal": "*",

            "Action": [

                "s3:ListBucket",

                "s3:GetObject"

            ],

            "Resource": [

                "arn:aws:s3:::dev-sdp-web-components",

                "arn:aws:s3:::dev-sdp-web-components/*"

            ],

            "Condition": {

                "StringEquals": {

                    "aws:SourceVpce": "vpce-0abe999b3b9bcb509"

                }

            }

        },

        {

            "Sid": "DenyAccessFromUnknownSource",

            "Effect": "Deny",

            "Principal": "*",

            "Action": [

                "s3:ListBucket",

                "s3:GetObject"

            ],

            "Resource": [

                "arn:aws:s3:::dev-sdp-web-components",

                "arn:aws:s3:::dev-sdp-web-components/*"

            ],

            "Condition": {

                "NotIpAddress": {

                    "aws:SourceIp": [

                        "65.132.246.78/32",

                        "112.216.0.186/32",

                        "44.230.99.176/32",

                        "13.124.181.22/32",

                        "210.94.41.88/32",

                        "210.94.41.89/32",

                        "203.126.64.64/32",

                        "203.126.64.65/32",

                        "203.126.64.66/32",

                        "203.126.64.67/32",

                        "203.126.64.68/32",

                        "203.126.64.69/32",

                        "203.126.64.70/32",

                        "203.126.64.71/32",

                        "52.32.215.165/32",

                        "52.25.159.9/32",

                        "101.96.101.244/32",

                        "203.205.17.206/32",

                        "220.85.58.253/32"

                    ]

                },

                "StringNotEquals": {

                    "aws:SourceVpce": [

                        "vpce-0ff3b4a91a7872243",

                        "vpce-01f2889f59a2af828",

                        "vpce-0abe999b3b9bcb509"

                    ]

                },

                "BoolIfExists": {

                    "aws:PrincipalIsAWSService": "false"

                }

            }

        }

    ]

}