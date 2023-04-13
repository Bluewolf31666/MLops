FROM jupyter/scipy-notebook


WORKDIR .

ADD requirements.txt /app/
  
RUN pip install --upgrade pip  
RUN pip install -r /app/requirements.txt

CMD jupyter notebook --port=8888 --no-browser --ip=0.0.0.0 --allow-root
