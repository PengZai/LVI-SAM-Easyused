# Author: TaeYoung Kim
# email: tyoung96@yonsei.ac.kr

FROM tyoung96/ros-melodic-cudagl11.2:latest

RUN apt-get install -y curl gnupg2 \
    && curl -sSL 'https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc' | apt-key add -
  


# Install PCL & Eigen & essential libraries
RUN apt-get update && apt-get install -y cmake \
    libeigen3-dev libpcl-dev libatlas-base-dev \
    libgoogle-glog-dev libsuitesparse-dev libglew-dev

# Install ceres-solver
WORKDIR /root/thirdParty
RUN wget https://github.com/ceres-solver/ceres-solver/archive/refs/tags/1.14.0.tar.gz
RUN tar zxf 1.14.0.tar.gz
WORKDIR /root/thirdParty/ceres-solver-1.14.0

RUN mkdir build && cd build && cmake -DCMAKE_BUILD_TYPE=Release ../../ceres-solver-1.14.0 && make -j2 && make install

RUN apt-get install python3-pip -y
RUN python3 -m pip install evo

# Install GTSAM
WORKDIR /root/thirdParty
RUN wget -O gtsam.zip https://github.com/borglab/gtsam/archive/4.0.2.zip
RUN unzip gtsam.zip
WORKDIR /root/thirdParty/gtsam-4.0.2
RUN mkdir build && cd build && cmake -DGTSAM_BUILD_WITH_MARCH_NATIVE=OFF .. && make -j2 && make install

WORKDIR /root/catkin_ws

# Load ROS environment at each run
COPY ./ros_entrypoint.sh /
RUN chmod 755 /ros_entrypoint.sh
ENTRYPOINT ["/ros_entrypoint.sh"]

RUN echo "source /opt/ros/melodic/setup.bash" >> /root/.bashrc
RUN echo "source /root/catkin_ws/devel/setup.bash" >> /root/.bashrc


CMD ["bash"]
