# PASSO A PASSO INSTALAÇÃO DO PACOTE PX4-AUTOPILOT
Todo o procedimento foi feito com o ROS-Noetic no Ubuntu 20.04, junto com Gazebo 11 ( vem automaticamente quando se instala o pacote ROS-Noetic na versão full desktop).

## Clonar o PX4-Autopilot

     git clone https://github.com/PX4/PX4-Autopilot.git --recursive

     bash ./PX4-Autopilot/Tools/setup/ubuntu.sh

   Logo após, reinicie o seu computador manualmente ou utilizando o comando no terminal:
   
     sudo reboot

## Instalação MAVROS

     mkdir -p ~/catkin_ws/src

     cd ~/catkin_ws

     catkin init 
 
     wstool init src
 
#### Em catkin_ws/src/

     rosinstall_generator --rosdistro kinetic mavlink | tee /tmp/mavros.rosinstall
 
     rosinstall_generator --upstream mavros | tee -a /tmp/mavros.rosinstall

#### Em catkin_ws/

     wstool merge -t src /tmp/mavros.rosinstall

     wstool update -t src -j4

     rosdep install --from-paths src --ignore-src -y

     catkin build

     source devel/setup.bash
     
O comando catkin build pode não estar instalado junto com o seu ROS por se tratar de um comando mais antigo,para isso instale-o digitando:
   
      sudo apt update
      sudo apt install python3-catkin-tools

### Agora, use o comando cd e logo em seguida dê nano ~/.bashrc e adicione no final:

     source ~/catkin_ws/devel/setup.bash

     source ~/PX4-Autopilot/Tools/simulation/gazebo-classic/setup_gazebo.bash ~/PX4-Autopilot ~/PX4-Autopilot/build/px4_sitl_default

     export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/PX4-Autopilot

     export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/PX4-Autopilot/Tools/simulation/gazebo-classic/sitl_gazebo-classic

     export GAZEBO_PLUGIN_PATH=$GAZEBO_PLUGIN_PATH:/usr/lib/x86_64-linux-gnu/gazebo-11/plugin
 
#### Salve a modificação dando Ctrl-X e logo em seguida dê o comando source .bashrc para atualizar .bashrc

#### Antes de continuar, dê cd (caso esteja em algum arquivo) e digite:

     cd PX4-Autopilot

     make px4_sitl gazebo-classic_iris_opt_flow

#### O comando anterior instala o modelo do drone e é necessário para dar os próximos comandos.

### Logo após abra 3 terminais diferentes:

#### No primeiro:

      roslaunch px4 px4.launch
     
#### No segundo 

      roslaunch gazebo_ros empty_world.launch world_name:=$(pwd)/Tools/simulation/gazebo-classic/sitl_gazebo-classic/worlds/empty.world
     
#### No terceiro

      rosrun gazebo_ros spawn_model -sdf -file $(pwd)/PX4-Autopilot/Tools/simulation/gazebo-classic/sitl_gazebo-classic/models/iris_opt_flow/iris_opt_flow.sdf -model iris -x 0 -y 0 -z 0 -R 0 -P 0 -Y 0
     
     
#### Explicação:
 1-No primeiro terminal é iniciado o PX4 junto com o mavlink. 
 
 2-No segundo é iniciado a simulação do gazebo em um mundo escolhido contido no repositório do PX4-Autopilot,no caso está setado para um mundo default empty_world.
 
 3-No terceiro é setado o modelo a ser utilizado,no caso é o iris_opt_flow , e a sua posição de spawn.
  
#### Extra: caso tenha o QGroundControl instalado no computador dê:
      
       ./QGroundControl.AppImage
      
 e ele automaticamente irá linkar o px4 e a partir dele pode-se utilizá-ló para controlar o drone na simulação do gazebo.      
 


     

