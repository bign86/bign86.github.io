---
title: nfpy Project installation
description: ""
date: 2025-10-19
preview: ""
draft: true
tags:
    - python
    - flask
    - nginx
    - vuejs
categories:
    - projects
slug: nfpy_install
---


Python env

- create
sudo python -m venv /opt/nfpyvenv
sudo chown -R nero /opt/nfpyvenv



Cutils

- compile
- install
cp cutils.cpython-311-aarch64-linux-gnu.so /opt/nfpyvenv/lib64/python3.11/site-packages/
cp -r cutils.egg-info/ /opt/nfpyvenv/lib64/python3.11/site-packages/

nfpy

- build
python -m build

- install
cd dist
scp nfpy-0.49-py3-none-any.whl nero@pi.local:~/
source /opt/nfpyvenv/bin/activate
pip3 install nfpy-0.49-py3-none-any.whl

flask

- gunicorn
pip3 install gunicorn

- install
copy server/ files main.py and src/

- start
cd /opt/nfpyUI/server
gunicorn -b 0.0.0.0:8080 main:app

vue

- build
npm run build

- install
scp -r dist/* nero@pi.local:/opt/nfpyUI/client

nginx

- install
sudo apt install nginx

- configure
sudo /etc/nginx/sites-available/default
change to the root folder

- start
sudo systemctl restart nginx


