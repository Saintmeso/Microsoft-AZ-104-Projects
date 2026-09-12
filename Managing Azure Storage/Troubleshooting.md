## Problems I ran into

This project was quite simple so there wasn't much problems I ran into but one thing I wanted to point out specifically was my mistake when configuring the security group + networking blade in the storage account.
When configuring the Storage account, I turned off public networking so only outbound access was allowed rather than inbound.

What my mistake was that, I forgot to add my IPV4 address. Since I forgot my IPV4 address, When it came to creating the blob container and trying to upload anything inside of there, I had no permissions to do so.
So to Troubleshoot this, I went back into the Storage group configurations and went to networking.
After that I Added my IPV4, I then went back to my blob container to ensure I had access and I successfully did.

This is just an example of a simple mistake I made when configuring this project.
