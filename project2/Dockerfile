# Base Image
FROM python:3.11-slim

# set working path
WORKDIR /app

COPY app/requirements.txt .

RUN pip install -r requirements.txt

COPY app/  .
 
CMD ["python","app.py"]

EXPOSE 5000


# python app setup on rhel10 using your dockerfile 