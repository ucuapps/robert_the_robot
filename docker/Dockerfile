FROM shadowrobot/build-tools:xenial-kinetic-ide

MAINTAINER "ATP&UCU"

LABEL Description="This ROS Kinetic image containes all dependencies for Robert project" Vendor="Lyubomyr" Version="1.0"

ENV remote_deploy_script="https://raw.githubusercontent.com/shadow-robot/sr-build-tools/$toolset_branch/ansible/deploy.sh"



RUN set -x && \
    \
    apt-get update -y && \
    apt-get upgrade -y --no-install-recommends && \
    \
    apt-get install -y ros-kinetic-rosserial-arduino \
    ros-kinetic-rosserial-python  \
    ros-kinetic-rosserial-server  \
    ros-kinetic-rosserial-client  \
    ros-kinetic-rosserial-msgs && \
    \
    echo "Removing workspace folder" && \
    rm -rf /home/$MY_USERNAME/workspace && \
    \    
    echo "Running one-liner" && \
    wget -O /tmp/oneliner "$( echo "$remote_deploy_script" | sed 's/#/%23/g' )" && \
    chmod 755 /tmp/oneliner && \
    gosu $MY_USERNAME /tmp/oneliner -o ucuapps -r robert_the_robot -b master -v kinetic && \
    \
    echo "Sourcing workspace in .bashrc" && \
    echo 'source /home/$MY_USERNAME/workspace/robert_the_robot/base/devel/setup.bash' >> /home/$MY_USERNAME/.bashrc && \
    \
    echo "Removing cache" && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /home/$MY_USERNAME/.ansible /home/$MY_USERNAME/.gitconfig

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

CMD ["/usr/bin/terminator"]
