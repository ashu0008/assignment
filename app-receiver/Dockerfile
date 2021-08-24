####################################################
FROM python:3.7 as test_image


ENV SERVER_IP="0.0.0.0"
ENV SERVER_PORT="8080"
ENV OUTPUT_DIR="/usr/src/app-receiver/output"
ENV DECRYPTION_KEY="/run/secrets/decryption_key"

WORKDIR /app
COPY Pipfile ./
COPY Pipfile.lock ./
RUN pip install --no-cache-dir pipenv
RUN pipenv install --dev
COPY . . 
RUN  /root/.local/share/virtualenvs/*/bin/pytest -v tests


# Production image, we don't need test cases to be here 
FROM python:3.7 as production_image

ENV SERVER_IP="0.0.0.0"
ENV SERVER_PORT="8080"
ENV OUTPUT_DIR="/usr/src/app-receiver/output"
ENV DECRYPTION_KEY="/run/secrets/decryption_key"

EXPOSE 8080

COPY Pipfile ./
COPY Pipfile.lock ./
RUN pip install --no-cache-dir pipenv
RUN pipenv install --system --deploy --ignore-pipfile



COPY ./receiver ./receiver/
ENV GUNICORN_CMD_ARGS="--bind=0.0.0.0 --chdir=./receiver/"
RUN mkdir -p /usr/src/app-receiver/output && mkdir -p /run/secrets/  && touch /run/secrets/decryption_key


ENTRYPOINT ["gunicorn", "server:app"]
