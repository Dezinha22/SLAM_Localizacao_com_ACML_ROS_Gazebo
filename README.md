# SLAM_Localizacao_com_ACML_ROS_Gazebo

Repositório com as etapas e os arquivos necessários para a aplicação do ACML com os algoritmos gmapping e hector_slam.

A base de dados principal é obtida a partir do input de dados provenientes do ambiente simulado do [LaR - UFBA](https://github.com/Dezinha22/SLAM_Localizacao_com_ACML_ROS_Gazebo),o clearpath husky e o sensor LiDAR 2D

## Configuração de Start-up

### Pré-Requisitos de Sistemas

1 - Utilize a distribuição **Ubuntu 24.04 LTS**;
    
2 - Instale o **Docker 29.5.3**;

3 - Possua acesso aos navegadores **Google Chrome**, **Mozilla Firefox** ou similares.

### Pré-Requisitos de Arquivos

4 - Faça a transferência dos dados de simulação contidos no [google drive](https://drive.google.com/drive/folders/1lF4HExtL49w9botrIfjxp7nfLHgtEdTF?usp=drive_link).

5 - Faça a transferência do pacote com os [arquivos de launch ](https://github.com/Dezinha22/SLAM_Localizacao_com_ACML_ROS_Gazebo) contido neste repositório.

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

14 - Caso apresente a mensagem de erro *! Image l... pull access denied for lar-gazebo, repository does not exist or may require 'docker login' 31.1s*. Não se assuste, isso costuma ser decorrente do fato do repositório não ter sido localizada na busca. Esse fato não altera o sucesso do pipeline, uma vez que ele será executado com os arquivos locais (obtidos após clonar o repositório do github).

15 - Utilize o comando *docker compose ps* para validar a criação do ambiente. A saída deve ser similar a exposta seguir:

    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~/lar_gazebo$ docker compose ps
    NAME                IMAGE               COMMAND                  SERVICE      CREATED          STATUS          PORTS
    lar_gazebo_noetic   lar-gazebo:noetic   "/ros_entrypoint_lar…"   lar_gazebo   22 minutes ago   Up 22 minutes   

16 - Acesse o container recém-criado pelo comando *docker compose exec lar_gazebo bash*. A saída do comando deve ser similiar a:

    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~/lar_gazebo$ docker compose exec lar_gazebo bash
    ros@docker-desktop:/ws$ 

17 - Utilize o comando *pw* para validar o local onde está navegando. Ele deve retornar o seguinte:

    ros@docker-desktop:/ws$ pwd
    /ws

18 - Após esse processo, utilize o comando *ls* para localizar, no dispositivo local (notebook, desktop, etc.) a bag extraída desse [repositório](https://github.com/Dezinha22/SLAM_Localizacao_com_ACML_ROS_Gazebo). Importante, você pode ajustar as atribuições contidas no comando para otimizar a busca. Assim como, a localização do arquivo também pode ser obtida utilizando os comandos contidos no item 7 desse tutorial. O comando deve ser executado em outra aba do terminator, executado diretamente no ambiente local e a saida deve ser algo similiar a:

    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~/lar_gazebo$ ls ~/Desktop/ | grep husky
    husky_Kalmanan_fusion

19 - Nessa etapa você realizará a importação do pacote ROS para o container docker. Para isso, execute o comando docker cp *~/local_do_arquivo_de_origem:/ws/src/local_de_destino* fora do ambiente do docker. A saida será similar a: 

    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~/Desktop/Atividade_II_Localização_Robotica$ docker cp ~/Desktop/Atividade_II_Localização_Robotica/husky_Kalmanan_fusion_backup lar_gazebo_noetic:/ws/src/husky_Kalmanan_fusion
    Successfully copied 9.98kB (transferred 21kB) to lar_gazebo_noetic:/ws/src/husky_Kalmanan_fusion

20 - Agora, verifique se o arquivo efetivamente foi transferido para o container docker em execução. Para isso, utilize o comando *cd*, *pwd*, dentre outros direcionados a localização e compartilhamento de endereço de pastas/arquivos. Por exemplo, temos o comando  *cd /ws/src/local_de_destino* para acessar o pacote ROS nomeado husky_part3_20260629. A saida no terminal deve ser semelhante a:

    ros@docker-desktop:/ws/src/husky_part3_20260629$ 

21 - Nesse momento, verifique os arquivos contidos na pasta copiada para confirmar que a transferência foi realizada com sucesso. Para isso, utilize o *ls* para verificar os arquivos contidos dentro da bag. A estrutura deve ser parecida com:

    ros@docker-desktop:/ws/src/husky_Kalmanan_fusion$ ls
    CMakeLists.txt  config  launch  package.xml  scripts

22 - Agora, retorne a raiz do workspace e compile o pacote ROS. Para isso, utilize o comando *catkin build nome_do_arquivo* ou similar. A saída da execução desse comando será parecida com: 

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

## Execução

O processo de execução é composto por três etapas. A primeira parte é responsavél por gerar os mapas de acordo com os algoritmos e consiste em três pilares principais. Com a segunda parte responsavél pela aplicação do AMCL e obtenção dos gráficos e das métricas.

### Execução | Primeira Parte

De forma macro, a primeira parte pode ser dividida em:

A) Execução do arquivo launch correspondente para geração do mapa de acordo com algoritmo selecionado (ex: Hecktor_Slam ou Gmapping);

B) Execução da bag com os dados da simulação; 

C) Execução do script para salvar o mapa e os demais dados oriundos da execução do arquivo launch em conjunto com a bag. 

Para o processo em tela, é imprescindível que os dados sejam lidos quando o processo de construção do mapa esteja ativo. Além disso, uma vez finalizada a execução da Bag, deve ser feita a execução do comando para salvar os mapas e demais informações oriundas do processamento finalizado. 

Dessa forma, se faz necessário ter diversos terminais abertos simultaneamente. Portanto, recomendo utilizar o terminator e realize o processo a seguir:

25 - Utilize o comando *cd* para acessar a pasta clonada do Github do LaR UFBA e, dentro da pasta, execute o comando *docker compose exec lar_gazebo bash* para acessar o mesmo container criado. Seguido pelo comando source devel/setup.bash. Para melhor entendimento dessa etapa, iremos chamar esse primeiro terminal de terminal Alpha. A saida deve ser similar a essa:

    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~$ cd ~/lar_gazebo
    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~/lar_gazebo$ docker compose exec lar_gazebo bash
    ros@docker-desktop:/ws$ 
    ros@docker-desktop:/ws$ source devel/setup.bash
    ros@docker-desktop:/ws$ 

26 - No terminal Alpha, recomendo executar o comando *roscore* para garantir a execução do ROS. A saida deverá ser semelhante a essa:

    (base) matheus@matheus-VivoBook-ASUSLaptop-X512FBC-X512FBC:~/lar_gazebo$ docker compose exec lar_gazebo bash
    ros@docker-desktop:/ws$ roscore
    ... logging to /home/ros/.ros/log/83ab5690-7045-11f1-b278-525400123456/roslaunch-docker-desktop-487.log
    Checking log directory for disk usage. This may take a while.
    Press Ctrl-C to interrupt
    Done checking log file disk usage. Usage is <1GB.

    started roslaunch server http://docker-desktop:58241/
    ros_comm version 1.17.4


    SUMMARY
    ========

    PARAMETERS
     * /rosdistro: noetic
     * /rosversion: 1.17.4

    NODES

    auto-starting new master
    process[master]: started with pid [495]
    ROS_MASTER_URI=http://docker-desktop:11311/

    setting /run_id to 83ab5690-7045-11f1-b278-525400123456
    process[rosout-1]: started with pid [505]
    started core service [/rosout]

27 - Mantenha o terminal Alpha ativo e, em um segundo terminal, execute os mesmos passos do item 25. Esse novo terminal chamaremos de terminal Bravo.

28 - No terminal Bravo iremos ativar a construção do mapa (launch) seguindo o script do Hector_Slam ou do Gmapping. Ele será responsável por realizar o mapeamento a partir dos dados informados pela Bag. Para isso, exeute o comando *roslaunch husky_part3_20260629 slam_gmapping_husky.launch*. A saída do comando deverá ser semelhante a:

       ros@docker-desktop:/ws$ roslaunch husky_part3_20260629 slam_gmapping_husky.launch
    ... logging to /home/ros/.ros/log/a59ea058-741a-11f1-965d-525400123456/roslaunch-docker-desktop-4141.log
    Checking log directory for disk usage. This may take a while.
    Press Ctrl-C to interrupt
    Done checking log file disk usage. Usage is <1GB.

    started roslaunch server http://docker-desktop:58611/

    SUMMARY
    ========

    PARAMETERS
     * /rosdistro: noetic
    * /rosversion: 1.17.4
     * /slam_gmapping/angularUpdate: 0.2
     * /slam_gmapping/base_frame: base_link
     * /slam_gmapping/delta: 0.05
     * /slam_gmapping/linearUpdate: 0.3
     * /slam_gmapping/map_frame: map
     * /slam_gmapping/map_update_interval: 2.0
     * /slam_gmapping/maxUrange: 15.0
     * /slam_gmapping/minimumScore: 150
     * /slam_gmapping/odom_frame: odom
     * /slam_gmapping/particles: 100
     * /slam_gmapping/xmax: 30
     * /slam_gmapping/xmin: -30
     * /slam_gmapping/ymax: 30
     * /slam_gmapping/ymin: -30

    NODES
      /
    slam_gmapping (gmapping/slam_gmapping)

    ROS_MASTER_URI=http://localhost:11311

    process[slam_gmapping-1]: started with pid [4162]


29 - Mantenha os terminais Alpha e Bravo ativos e abra o terminal Charle. Ele será responsável por rodar a Bag. Para isso, repita o processo do item 25 e em seguida execute o comando *rosbag play /ws/2026-06-16-00-36-58-001.bag --clock*. 

30 - Quando finalizar a execução da Bag no terminal Charle, abra o terminal Echo e repita o processo do item 25. Após isso, execute o comando *rosrun map_server map_saver -f /ws/src/husky_part3_20260629/maps/mapa_gmapping* ou *rosrun map_server map_saver -f /ws/src/husky_part3_20260629/maps/hector_slam*. Ele será responsável por salvar os mapas gerados.


### Execução | Segunda Parte

A segunda parte será responsável pela análise de qualidade do mapa com os parâmetros de AMCL. Para isso, siga as seguintes etapas. 

31 - Mantenha o terminal Alpha ativo. 

32 - No terminal Bravo execute o launch do AMCL com o comando *roslaunch husky_part3_20260629 amcl_gmapping.launch*. A saida deve ser similar a:

33 - No terminal Echo grave a comparação realizada pelo launch da AMCL. Nesse caso, utilize o comando: 

rosbag record -O /ws/src/husky_part3_20260629/bags/amcl_gmapping_comparison.bag \
/amcl_pose \
/gazebo_ground_truth/odom \
/tf \
/tf_static

A saída deve ser similar a:


34 - No terminal Charle, execute a Bag pelo comando *rosbag play /ws/2026-06-16-00-36-58-001.bag --clock*. 

35 - Quando finalizar a execução da Bag, finalize a gravação no terminal Echo (utilize o comando crtl + C) e utilize o comando *ls -lh /ws/src/husky_part3_20260629/bags/amcl_gmapping_comparison.bag*
para verficar se foi efetivamente gravada. A saída deve ser semelhante a essa:

ros@docker-desktop:/ws$ ls -lh /ws/src/husky_part3_20260629/bags/amcl_gmapping_comparison.bag
-rw-r--r-- 1 ros ros 5.8M Jun 30 01:21 /ws/src/husky_part3_20260629/bags/amcl_gmapping_comparison.bag

36 - Após essa etapa, execute - no terminal Echo - o script em python que será responsável por gerar o mapa e a trajetória do AMCL. O comando necessário para isso é o *python3 /ws/src/husky_part3_20260629/scripts/plot_gmapping_trajectory.py*. A saida deve ser semelhante a: 

    ros@docker-desktop:/ws$ python3 /ws/src/husky_part3_20260629/scripts/plot_gmapping_trajectory.py
    === Gerando visualização profissional - Gmapping ===

    Carregando mapa...
    Extraindo trajetória...
    Gerando figura de alta qualidade...
    Figura salva em: /ws/src/husky_part3_20260629/figures/gmapping/mapa_gmapping_trajetoria_20260630_012806.png

    Concluído com sucesso!

### Execução | Terceira Parte

Nessa etapa, iremos extrair as métricas do script.

37 - Agora, no terminal Echo, execute o comando *python3 /ws/src/husky_part3_20260629/scripts/analise_gmapping_estabilidade.py* para extrair as métricas. A saída deve ser semelhante a:

    ros@docker-desktop:/ws$ python3 /ws/src/husky_part3_20260629/scripts/analise_gmapping_estabilidade.py
    === Análise com Métricas de Estabilidade - Gmapping ===

    AMCL: 103 | GT: 2540
    Sincronizados: 103

    Análise concluída!
    Resultados salvos em: /ws/src/husky_part3_20260629/analysis/gmapping/20260630_014102


## Conclusão

As métricas de estabilidade revelaram que os dados analisados possuem muita instabilidade. Isso fica ainda mais evidente ao verificarmos que a média dos "saltos" entre as poses é de 0.1682 m e observarmos que a máxima dos saltos é de 0.2538 m. Essa informação é ainda mais agravante quando recordamos do fato de estarmos analisando o grupo de 103 poses dentro do universo de 2540 poses de ground_truth, 5080 de front/scan e 5080 de odometria.

Essa disparidade pode ter sua causa justamente nos inúmeros Timestamp de qualidade inadequada para a simmulação. Isso resultou em sincronismo deficiente, afetou diretamente o Transform Frames - TF e é um dos principais responsáveis pelo feedback de insucesso diante das tentativas em utilizar o Hector Slam (. Justificando, por sua vez, o relativo sucesso em alcançar representações com o script Gmapping demonstrando maior robustez para este conjunto de dados.

Em relação ao erro final de posição ser da ordem de 3.5496 m e do RMSE Yaw ser de 2.7538 rad, temos que considerar que a bag foi obtida sem sincronismo inicial ao que é considerado o ponto de referência inical do Husky dentro do mundo simulado no Gazebo. Além disso, não houve uma bag com a execução da simulação respeitando a referência, portanto, não houve como validar essa argumentação. Sendo permanecida como uma hipótese. 

Além disso, o Husky apresentou saltos, escorregamentos e demais variações durante o regime de funcionamento no ato da gravação da bag. Esses comportamentos não somente impactam na leitura dos sensores, como também tornam os dados e as métricas oriundas dos sensores interoceptivos simplesmente infrutiferas. Esse fato foi verificado principalmente quando se foi realizado as atividades de fusão sensorial contida nesse [link](https://github.com/Dezinha22/Fus-o_Sensorial-Filtro_Kalman_ROS). Onde, a aplicação do filtro de Kalman com o uso da odometria + IMU + GPS apresentou o resultado mais satisfatório.

Sobre o mapa gerado, temos uma imagem com paredes bem destacadas, poucos obstáculos falsos e algumas regiões onde existe falhas na representação das paredes. Além, de alguns objetos que não foram identificados. Esse comportamneto pode possuir como uma das causas justamente a questão do uso do sensor LiDAR 2D que possui limitações decorrentes da natureza do seu funcionamneto (varredura) aliado com os próprios desafios presentes no mapeamento de ambientes com sensores LiDAR (inclusive, decorrentes da opacidade de elementos presentes no ambiente a ser mapeado. Assim como, por se utilizar da estratégia do filtro de particulas com odometria + sensor LiDAR, é previsivél que existam falsos positivos e falsos negativos decorrentes da própria natureza probabilistica do modelo.

Em relação a trajetória, o caminho percorrido ficou muito bem representado. Conseguindo reparar oscilações presentes no Ground Truth. A AMCL em conjunto com o script do Gmapping fez um ótimo trabalho nessa questão. Sendo a questão relacionada ao erro de posição mais uma vez elucidando as instabilidades da medição. Com a aparente estabilidade inicial podendo ser resultado tão somente da incapacidade de conseguir representar as poses iniciais. Com a própria oscilação vertiginosa presente aos aproximadamente 25 sendo um forte indicio.

Não suficiente, as próprias limitações computacionais e os diversos erros/bugs também possuem forte possibilidade de impacto nos resultados. Afinal, essas métricas e essas leituras foram feitas em um dispositivo eletrônico, com as váriaveis que estamos considerando como distância, tempo e representação gráfica sendo literalmente sinais elétricos que foram tradados e processados. Com potencial de afetar diretamente na interpretação. 

Diante disso, para evitar manipulações, interpretações equivocadas e possibilitar conclusões mais técnicas sobre o motivo desses erros, dessas instabilidades e dos insucessos relatods. Se faz imperativo realizar novos testes em toda a etapa do ciclo de vida das atividades. Da coleta de dados no Gazebo até a emissão das métricas de qualidade. Aplicando etapas de verificação e validação em cada subprocesso. Com intuito de efetivamente ter conclusões concretas e auditavéis. 

As imagens obtidas e as métricas de desempenho podem ser melhor visualizadas na pasta husky_part3_gmapping contida nesse [repositório](https://github.com/Dezinha22/SLAM_Localizacao_com_ACML_ROS_Gazebo).

Por fim, segue a imagem do erro que persistiu durante diversas tentativas de confecção do Hecktor Slam.

![Erro na Imagem](https://github.com/Dezinha22/SLAM_Localizacao_com_ACML_ROS_Gazebo/blob/main/Screenshot%20from%202026-06-29%2022-49-00.png)





