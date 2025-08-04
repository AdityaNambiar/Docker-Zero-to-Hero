FROM ubuntu AS basebuild

WORKDIR /app

COPY devops /app/
COPY . /app/    

RUN apt-get update && apt-get install -y python3 python3-pip python3-venv

RUN python3 -m venv /app/venv1

# Use a second stage to create a venv with django and other dependencies
FROM basebuild AS buildvenv
COPY requirements.txt /app/requirements.txt
RUN /app/venv1/bin/pip install --no-cache-dir -r /app/requirements.txt

# Use a distroless image for the final build
FROM gcr.io/distroless/python3-debian12


COPY --from=basebuild /app /app
COPY --from=buildvenv /app/venv1 /app/venv1

WORKDIR /app

EXPOSE 8000

ENV PYTHONPATH=/app/venv1/lib/python3.12/site-packages

ENTRYPOINT ["/app/venv1/bin/python3"] 

CMD  ["manage.py","runserver","0.0.0.0:8000"]



