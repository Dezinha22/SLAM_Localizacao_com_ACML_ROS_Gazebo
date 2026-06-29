# SLAM_Localizacao_com_ACML_ROS_Gazebo

Repositório com as etapas e os arquivos necessários para a aplicação do ACML com os algoritmos gmapping e hector_slam.

A base de dados principal é obtida a partir do input de dados provenientes do ambiente simulado do LaR - UFBA, clearpath husky e sensor LiDAR 2D.

# Fusao_Sensorial-Filtro_Kalman_ROS
Repositório com as etapas e os arquivos necessários para a aplicação do filtro Kalman.

A base de dados principal é obtida a partir do input de dados provenientes do ambiente simulado do [LaR - UFBA](https://github.com/lar-deeufba/lar_gazebo) e o clearpath husky.

## Configuração de Start-up

### Pré-Requisitos de Sistemas

1 - Utilize a distribuição **Ubuntu 24.04 LTS**;
    
2 - Instale o **Docker 29.5.3**;

3 - Possua acesso aos navegadores **Google Chrome**, **Mozilla Firefox** ou similares.

### Pré-Requisitos de Arquivos

4 - Faça a transferência dos dados de simulação contidos no [google drive](https://drive.google.com/drive/folders/1lF4HExtL49w9botrIfjxp7nfLHgtEdTF?usp=drive_link).

5 - Faça a transferência do pacote com os [arquivos de launch ](https://github.com/Dezinha22/Fus-o_Sensorial-Filtro_Kalmanan_ROS/blob/main/%20Filtro%20de%20Kalmanan_Matheus_27_Jun_26.tar.gz) contido neste repositório.

### Primeiros Passos 

6 - Acesse o terminal (recomenda-se utilizar o _[terminator](https://gnome-terminator.org/)_);

7 - Localize os arquivos transferidos e reserve os seus respectivos endereços. Para localizá-los, pode utilizar o comando *find*, *locale*, *ls*, *pwd*, dentre outros. Consulte o [Tutorial A](https://www.hostinger.com/br/tutoriais/comandos-linux), [Tutorial B](https://info-ee.surrey.ac.uk/Teaching/Unix/index.html) ou similares para ter conhecimento de outros comandos e sua estrutura;

8 - Clone o repositório do [LaR - UFBA](https://github.com/lar-deeufba/lar_gazebo);

9 - Localize o local onde o repositório foi clonado e acesse a pasta. Para isso, pode utilizar os comandos *cd*, *find*, *locale*, *ls*, *pwd*, dentre outros (verifique o intem 7). Você deve chegar a algo similar a isso: 

    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~$ cd
    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~$ ls
    CoppeliaSim  google-chrome-stable_current_amd64.deb  Music      rosbags_env
    Desktop      lar_gazebo                              OpenPCDet  Templates
    Documents    miniconda3                              Pictures   Videos
    Downloads    Miniconda3-latest-Linux-x86_64.sh       Public
    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~$ cd lar_gazebo/
    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~/lar_gazebo$  

10 - Utilize o comando *ls* ou similar para verificar os arquivos contidos na pasta lar_gazebo e validar se a transferência do repositório foi satisfeita. A saída será similar a essa:

    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~$ cd lar_gazebo/
    (basqe) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~/lar_gazebo$ ls
    2026-06-16-00-36-58-001.bag  docker                LICENSE      README.md
    april_tags                   docker-compose.yml    maps         scripts
    CMakeLists.txt               husky_accessories.sh  models       worlds
    config                       husky_urdf_extras     Ola
    doc                          launch                package.xml
    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~/lar_gazebo$ pwd
    /home/matheus/lar_gazebo
    
11 - Incie o docker desktop. Cuidado ao utilizar o comando *systemctl --user start docker-desktop*. Ele pode crashar seu sistema operacional. Exigindo um logoff ou uma reinicialização forçada. Priorize a execução pelo atalho gerado no momento da instalação;

12 - Valide a inicialização do Docker pelo comando *sudo systemctl status docker*. Esse processo não precisa ser somente executado dentro da pasta lar_gazebo;

    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~/lar_gazebo$ sudo systemctl status docker
    ● docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: e>
     Active: active (running) since Mon 2026-06-22 22:46:04 -03; 1 day 3h ago
    TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
       Main PID: 1226 (dockerd)
          Tasks: 14
    Memory: 107.1M (peak: 144.4M swap: 10.5M swap peak: 11.4M)
        CPU: 10.835s
     CGroup: /system.slice/docker.service
             └─1226 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/cont>

13 - Crie o ambiente por meio do comando *docker compose up -d*. Esse comando será responsável por gerar o container e subir a imagem com os demais arquivos contidos no repositório clonado do github do LaR - UFBA. A saída esperada é a seguir: 

    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~/lar_gazebo$ docker compose up -d
    [+] up 1/1
     ! Image l... pull access denied for lar-gazebo, repository does not exist or may require 'docker login' 31.1s
    [+] Building 13.9s (20/20) FINISHED                                             
     => [internal] load local bake definitions                                 0.0s
     => => reading from stdin 639B                                             0.0s
     => [internal] load build definition from Dockerfile.noetic                0.9s
     => => transferring dockerfile: 4.38kB                                     0.2s
     => [internal] load metadata for docker.io/osrf/ros:noetic-desktop-full    2.8s
     => [auth] osrf/ros:pull token for registry-1.docker.io                    0.0s
     => [internal] load .dockerignore                                          0.4s
     => => transferring context: 112B                                          0.0s
     => [ 1/12] FROM docker.io/osrf/ros:noetic-desktop-full@sha256:7dbfb9576d  0.5s
     => => resolve docker.io/osrf/ros:noetic-desktop-full@sha256:7dbfb9576d8e  0.4s
     => [internal] load build context                                          1.0s
     => => transferring context: 10.64kB                                       0.1s
     => CACHED [ 2/12] RUN apt-get update && apt-get install -y --no-install-  0.0s
     => CACHED [ 3/12] RUN if ! getent group "1000" >/dev/null; then groupadd  0.0s
     => CACHED [ 4/12] RUN mkdir -p /ws/src                                    0.0s
     => CACHED [ 5/12] WORKDIR /ws                                             0.0s
     => CACHED [ 6/12] COPY . /ws/src/lar_gazebo                               0.0s
     => CACHED [ 7/12] RUN source /opt/ros/noetic/setup.bash     && rosdep in  0.0s
     => CACHED [ 8/12] COPY docker/entrypoint.sh /ros_entrypoint_lar.sh        0.0s
     => CACHED [ 9/12] RUN chmod +x /ros_entrypoint_lar.sh                     0.0s
     => CACHED [10/12] RUN USER_HOME="$(getent passwd "ros" | cut -d: -f6)"    0.0s
     => CACHED [11/12] RUN USER_HOME="$(getent passwd "ros" | cut -d: -f6)"    0.0s
     => CACHED [12/12] WORKDIR /ws                                             0.0s
     => exporting to image                                                     3.0s
     => => exporting layers                                                    0.0s
     => => exporting manifest sha256:6489891325da281a48fe69a9146ad18512cee46e  0.2s
     => => exporting config sha256:2d892a6bff95c9db42228b1fc2f11f1556a6da4f9e  0.1s
     => => exporting attestation manifest sha256:0a71f638aa78964dfa808dab860f  1.0s
     => => exporting manifest list sha256:a2dc961537a9cdf1061cbe78a799a97b1c1  0.5s
     => => naming to docker.io/library/lar-gazebo:noetic                       0.2s
    [+] up 2/2acking to docker.io/library/lar-gazebo:noetic                    0.2s
     ✔ Image lar-gazebo:noetic     Built                                       45.5s
     ✔ Container lar_gazebo_noetic Started                                      8.8s

    What's next:
        Filter, search, and stream logs from all your Compose services
        in one place with Docker Desktop's Logs view. docker-desktop://dashboard/logs?appId=lar_gazebo

14 - Caso apresente a mensagem de erro *! Image l... pull access denied for lar-gazebo, repository does not exist or may require 'docker login' 31.1s*. Não se assuste, isso costuma ser decorrente do fato do repositório não ter sido localizada na busca. Esse fato não altera o sucesso do pipiline, uma vez que ele será executado com os arquivos locais (obtidos após clonar o repositório do github).

15 - Utilize o comando *docker compose ps* para validar a criação do ambiente. A saída deve ser similar a esposta seguir:

    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~/lar_gazebo$ docker compose ps
    NAME                IMAGE               COMMAND                  SERVICE      CREATED          STATUS          PORTS
    lar_gazebo_noetic   lar-gazebo:noetic   "/ros_entrypoint_lar…"   lar_gazebo   22 minutes ago   Up 22 minutes   

16 - Acesse o container recém-criado pelo comando *docker compose exec lar_gazebo bash*. A saída do comando deve ser similiar a:

    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~/lar_gazebo$ docker compose exec lar_gazebo bash
    ros@docker-desktop:/ws$ 

17 - Utilize o comando *pw* para validar o local onde está navegando. Ele deve retornar o seguinte:

    ros@docker-desktop:/ws$ pwd
    /ws

18 - Após esse processo, utilize o comando *ls ~/Desktop/ | grep husky* para localizar a bag extraída desse repositório. Importante, você pode ajustar as atribuições contidas no comando para otimizar a busca. Assim como, a localização do arquivo também pode ser alcançando utilizando os comandos contidos no item 7 desse tutorial. O comando deve ser executado em outra aba do terminator, executado diretamente no ambiente local e a saida deve ser algo similiar a:

    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~/lar_gazebo$ ls ~/Desktop/ | grep husky
    husky_Kalmanan_fusion

19 - Nessa etapa você realizará a importação do pacote ROS para o container docker. Para isso, execute o comando docker cp *~/Desktop/husky_Kalmanan_fusion lar_gazebo_noetic:/ws/src/husky_Kalmanan_fusion* fora do ambiente do docker. A saida será similar a: 

    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~/Desktop/Atividade_II_Localização_Robotica$ docker cp ~/Desktop/Atividade_II_Localização_Robotica/husky_Kalmanan_fusion_backup lar_gazebo_noetic:/ws/src/husky_Kalmanan_fusion
    Successfully copied 9.98kB (transferred 21kB) to lar_gazebo_noetic:/ws/src/husky_Kalmanan_fusion

20 - Agora, verifique se o arquivo efetivamente foi transferido para o container docker em execução. Para isso, utilize o comando *cd*, *pwd*, dentre outros direcionados a localização e compartilhamento de endereço de pastas/arquivos. Por exemplo, temos o comando  *cd /ws/src/husky_Kalmanan_fusion* para acessar a bag nomeada husky_Kalmanan_fusion. A saida no terminal deve ser semelhante a:

    ros@docker-desktop:/ws/src/husky_Kalmanan_fusion$ 

21 - Nesse momento, verifique os arquivos contidos na pasta copiada para confirmar que a transferência foi realizada com sucesso. Para isso, utilize o *ls* para verificar os arquivos contidos dentro da bag. A estrtura deve ser parecida com:

    ros@docker-desktop:/ws/src/husky_Kalmanan_fusion$ ls
    CMakeLists.txt  config  launch  package.xml  scripts

22 - Agora, retorne a raiz do workspace e compile o pacote ROS. Para isso, utilize o comando *catkin build husky_Kalmanan_fusion* ou similar. A saída da execução desse comando será parecida com: 

    ros@docker-desktop:/ws$ catkin build husky_Kalmanan_fusion
    --------------------------------------------
    Profile:                     default
    Extending:          [cached] /opt/ros/noetic
    Workspace:                   /ws
    --------------------------------------------
    Build Space:        [exists] /ws/build
    Devel Space:        [exists] /ws/devel
    Install Space:      [unused] /ws/install
    Log Space:          [exists] /ws/logs
    Source Space:       [exists] /ws/src
    DESTDIR:            [unused] None
    --------------------------------------------
    Devel Space Layout:          linked
    Install Space Layout:        None
    --------------------------------------------
    Additional CMake Args:       None
    Additional Make Args:        None
    Additional catkin Make Args: None
    Internal Make Job Server:    True
    Cache Job Environments:      False
    --------------------------------------------
    Buildlisted Packages:        None
    Skiplisted Packages:         None
    --------------------------------------------
    Workspace configuration appears valid.
    --------------------------------------------
    [build] Found 2 packages in 0.0 seconds.                      
    [build] Updating package table.                               
    Starting  >>> husky_Kalmanan_fusion                             
    Finished  <<< husky_Kalmanan_fusion                [ 13.6 seconds ]                            
    [build] Summary: All 1 packages succeeded!                                                   
    [build]   Ignored:   1 packages were skipped or are skiplisted.                              
    [build]   Warnings:  None.                                                                   
    [build]   Abandoned: None.                                                                   
    [build]   Failed:    None.                                                                   
    [build] Runtime: 13.8 seconds total.                                                         
    [build] Note: Workspace packages have changed, please re-source setup files to use them.

 23 - Nesse momento será necessário possuir a base de dados no mesmo local que está o ROS, o pacote ROS e os demais processos. Portanto, é necessário importar a BAG para o mesmo container docker. Nesse caso, pode utilizar o mesmo comando da etapa 22, aplicando os ajustes necessários em decorrência de serem arquivos com nomes diferente. A saber, o comando é *docker cp ~/lar_gazebo/2026-06-16-00-36-58-001.bag lar_gazebo_noetic:/ws/* e a saida deve ser semelhante a:

     (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~$ docker cp ~/lar_gazebo/2026-06-16-00-36-58-001.bag     lar_gazebo_noetic:/ws/
    Successfully copied 24.4GB (transferred 24.4GB) to lar_gazebo_noetic:/ws/

  24 - Após esse processo de importação da BAG (é natural demorar um pouco devido ao tamanho do arquivo), é importante verificar se de fato ela está localizada no docker. Para isso, execute o comando *ls /ws/*.bag* ou similar. A saida deve ser algo parecido com:

      ros@docker-desktop:/ws$ ls /ws/*.bag
    /ws/2026-06-16-00-36-58-001.bag
