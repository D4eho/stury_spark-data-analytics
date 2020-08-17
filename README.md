# 개요
Advanced Analytics with Spark 도서에 대한 스터디를 최신 버전의 스파크를 이용하여 다시 구현해보는 방식으로 진행해보고자 합니다. 저수준 API인 RDD는 가급적 사용하지 않으면서 DataFrame을 이용하여 같은 결과를 얻을 수 있도록 정리하면서 공부하는 것이 목적입니다.

# Windows 개발 환경 구성
본래 윈도우에서 개발을 진행하려면 아래와 같이 Spark 개발 환경이 필요합니다. 그 밖에도 Python이나 Hadoop, Jupyter와 같은 패키지를 추가로 설치해야 할 필요가 있습니다. 윈도우에서는 이런 패키지들을 하나하나 따로 받아서 설청해야 하는 과정들이 여간 귀찮습니다. 물론 Chocoletey를 사용한다면 좀 나을까요?

제가 아는 한, 이 모든 것을 한 번에 손쉽게 설치하는 방법은 Docker를 통해서 미리 구축된 이미지를 사용하는 것입니다.

* https://github.com/jupyter/docker-stacks
* https://hub.docker.com/r/jupyter/all-spark-notebook

이처럼 Jupyter에서 제공되는 Docker 이미지 중에서 편의상 all-spark-notebook을 이용하면 아래와 같은 명령어를 통해서 JupyterLab을 실행할 수 있습니다. 아래 명령어에서 <local_path> 부분에는 호스트 PC에서 Docker로 마운트할 경로를 지정해주면 됩니다. 이렇게 하면, 호스트 PC에서 Docker와 같은 볼륨을 사용하면서 파일을 손쉽게 주고 받을 수 있습니다. 또한, Docker 이미지를 추후 삭제한다고 하더라도 작업 내용이 그대로 남게 되고, 다음 이미지에서도 이어 받아서 사용할 수 있게 됩니다.

```
docker run -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes -v <local_path>:/home/jovyan --name jupyter jupyter/all-spark-notebook:f1811928b3dd
```

본 이미지에서 사용하는 라이브러리 버전은 다음과 같습니다. 

* Python - 3.8.5
* Spark - 3.0.0
* JupyterLab - 2.1.5

이후 Docker 이미지가 정상적으로 구동된다면, 아래와 같은 메시지를 확인할 수 있습니다.

```
Executing the command: jupyter lab
[I 15:54:08.611 LabApp] Writing notebook server cookie secret to /home/jovyan/.local/share/jupyter/runtime/notebook_cookie_secret
[I 15:54:08.885 LabApp] Loading IPython parallel extension
[I 15:54:09.269 LabApp] JupyterLab extension loaded from /opt/conda/lib/python3.8/site-packages/jupyterlab
[I 15:54:09.269 LabApp] JupyterLab application directory is /opt/conda/share/jupyter/lab
[I 15:54:09.273 LabApp] Serving notebooks from local directory: /home/jovyan
[I 15:54:09.273 LabApp] The Jupyter Notebook is running at:
[I 15:54:09.273 LabApp] http://5321dc62dd9d:8888/?token=2226f432eb94f5667b839c147a02db43d177d56cd9806f58
[I 15:54:09.273 LabApp]  or http://127.0.0.1:8888/?token=2226f432eb94f5667b839c147a02db43d177d56cd9806f58
[I 15:54:09.273 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 15:54:09.318 LabApp]

    To access the notebook, open this file in a browser:
        file:///home/jovyan/.local/share/jupyter/runtime/nbserver-6-open.html
    Or copy and paste one of these URLs:
        http://5321dc62dd9d:8888/?token=2226f432eb94f5667b839c147a02db43d177d56cd9806f58
     or http://127.0.0.1:8888/?token=2226f432eb94f5667b839c147a02db43d177d56cd9806f58
```

그러면, 마지막 주소를 인터넷 브라우저에 복사해서 JupyterLab에 접근할 수 있는데 그렇게 하지 마시고 아래 주소로 접근합니다.

```
http://localhost:8888/lab
```

이제 마지막의 주소의 토큰을 복사해서 하단의 패스워드 설정을 진행하신 다음에 그 패스워드를 이용해서 JupyterLab 에 접근하는 것이 더 좋습니다. 이렇게 해서 윈도우 재부팅 후에도 같은 Docker 이미지를 사용하여 로그인을 할 수 있습니다. 재부팅시에 Docker는 정지되어 있을 수 있는데, 이 때는 Docker 명령을 이용해서 실행하면 됩니다.

```
docker start jupyter
```
