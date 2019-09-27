FROM python:3.6.9-buster
# https://github.com/poteha/docker-nlp

RUN apt-get update && apt-get install -y unixodbc-dev gcc g++ apt-utils libapache2-mod-wsgi && \
    apt-get install -y unzip wget apt-transport-https

COPY requirements.txt /tmp/requirements.txt
RUN pip install --ignore-installed -r /tmp/requirements.txt && rm /tmp/requirements.txt

## Install JupyterLab extensions
#RUN apt-get install nodejs npm -y
#RUN jupyter labextension install @jupyterlab/toc
# Install table of content for Jupyter Notebook
RUN pip install jupyter_contrib_nbextensions && \
    jupyter contrib nbextension install --user && \
    jupyter nbextension enable toc2/main


# install latest version of fasttext
CMD apt-get update && apt-get install -y build-essential git unzip

RUN wget https://github.com/facebookresearch/fastText/archive/v0.9.1.zip && unzip v0.9.1.zip && cd fastText-0.9.1 && make && \
     git clone https://github.com/facebookresearch/fastText.git && cd fastText && pip install .

#CMD [
#     "wget", "https://github.com/facebookresearch/fastText/archive/v0.9.1.zip", "&&",
#     "unzip", "v0.9.1.zip", "&&",
#     "cd", "fastText-0.9.1", "&&",
#     "make", "&&",
#     "git", "clone", "https://github.com/facebookresearch/fastText.git", "&&",
#     "cd", "fastText", "&&",
#     "pip", "install", "."
# ]
#
#RUN git clone https://github.com/facebookresearch/fastText.git /tmp/fastText && \
#   rm -rf /tmp/fastText/.git* && \
#   mv /tmp/fastText/* / && \
#   cd / && \
#   make && pip install .

## DVC installation
#RUN wget https://dvc.org/deb/dvc.list -O /etc/apt/sources.list.d/dvc.list && \
#    apt-get install -y dvc

RUN pip install Flask


RUN useradd -m user -u 1000
RUN echo 'user:user' | chpasswd user

RUN apt-get update && apt-get install -y sudo

ARG GIT_CONFIG_USER_NAME
ARG GIT_CONFIG_EMAIL
WORKDIR /home/ai-hat

RUN echo "user ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/user && chmod 0440 /etc/sudoers.d/user
RUN chown -R user /home
USER user
RUN git config --global user.name $GIT_CONFIG_USER_NAME && git config --global user.email $GIT_CONFIG_EMAIL

CMD jupyter notebook --ip=0.0.0.0 --no-browser --allow-root --NotebookApp.token=''
#CMD python app/app.py

