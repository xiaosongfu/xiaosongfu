

https://www.cnrancher.com/



https://www.cnrancher.com/quick-start


docker run -d -v <主机路径>:/var/lib/rancher/ --restart=unless-stopped -p 80:80 -p 443:443 rancher/rancher:stable

```
[root@instance-iqwpq51e ~]# docker run -d -v /var/lib/rancher:/var/lib/rancher/ --restart=unless-stopped -p 80:80 -p 443:443 rancher/rancher:stable
Unable to find image 'rancher/rancher:stable' locally
stable: Pulling from rancher/rancher
35c102085707: Pull complete
251f5509d51d: Pull complete
8e829fe70a46: Pull complete
6001e1789921: Pull complete
190724123869: Pull complete
f144ec07b677: Pull complete
eb837ce35a44: Pull complete
71ab38270b07: Pull complete
d69cad2e554a: Pull complete
33d6cdb7458b: Pull complete
f5a0fc4d0fec: Pull complete
2bd480581d18: Pull complete
d81d5f05f9e8: Pull complete
Digest: sha256:b85f1dd239d2555ef438f46790642334e0c75f314a35047b93acb1b457b4cd09
Status: Downloaded newer image for rancher/rancher:stable
db24517bea47ad1f6514a48a86fa44263cc79c48ae74030ac79140a8c3c84d60
```
