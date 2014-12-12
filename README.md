Docker\_OpenSuSE\_11.3
====================
This repostiroy contains instructions that can assist with creating your own SuSE image.  This process was duplicated from the following site.

<a href="http://flavio.castelli.name/2013/04/12/docker-and-opensuse">SuSE instructions here</a>

The Docker image is available on Docker Hub <a href="https://registry.hub.docker.com/u/emccode/opensuse_11.3/">here</a>.

For this example, we used Boot2Docker to run the Docker daemon for us.

1. Start Boot2Docker
2. ```boot2docker ssh``` or run docker commands locally
3. Use KIWI to build an OpenSuse 11.3 Docker image with ```docker run --privileged -tid opensuse/kiwi```
5. ```docket attach <id>```
4. ```zypper --non-interactive install git```
5. ```git clone https://github.com/openSUSE/docker-containers```

Associate Zypper with the proper 12.3 repository.

1. ```cd docker-containers/openSUSE_13.1```
2. ```zypper removerepo zypper removerepo virtualization:appliances```
3. ```sudo zypper ar http://download.opensuse.org/repositories/Virtualization:/Appliances/openSUSE_12.3 virtualization:appliances```

Remove prior KIWI RPM to downgrade to a 12.3 version that works with 11.3.  This could be avoided likely by running the core opensuse-11.3 image.

1. ```rpm -e kiwi```
2. ```zypper install --repo virtualization:appliances kiwi kiwi-doc```

Update the config.xml file to replace 12.3 with 11.3

1. ```zypper install nano```
2. ```cd docker-containers/openSUSE_12.3```
3. ```nano config.xml```
4. ```touch ~/openSUSE_11_3_docker/root/etc/resolv.conf```

Start the process for creating the image.

1. ```/usr/sbin/kiwi --prepare . --root /tmp/openSUSE_11_3_rootfs```
2. ```tar cvjpf openSUSE_11_3_docker.tar.bz2 -C /tmp/openSUSE_11_3_docker_rootfs/```
3. This steps copies the file out of the container.  You can mount the docker image with -v hostpath:containerpath ahead of time to natively copy the file or place it in the container OS file system.  ```scp openSUSE_11_3_docker.tar.bz2 docker@172.17.42.1:/tmp/.``` username of ```docker``` and password of ```tcuser``` (if you didn't mount the volume)

Done!  Import the image into Docker!

1. ```docker import - emccode/opensuse_11.3:latest < /tmp/openSUSE_11_3_docker.tar.bz2```



Licensing
---------
Licensed under the Apache License, Version 2.0 (the “License”); you may not use this file except in compliance with the License. You may obtain a copy of the License at <http://www.apache.org/licenses/LICENSE-2.0>

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an “AS IS” BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

Support
-------
Please file bugs and issues at the Github issues page. For more general discussions you can contact the EMC Code team at <a href="https://groups.google.com/forum/#!forum/emccode-users">Google Groups</a>. The code and documentation are released with no warranties or SLAs and are intended to be supported through a community driven process.

Enjoy!
