FROM base-image:latest AS build-image

RUN apt install -y build-essential \
    python3.7-dev \
    python3-setuptools \
    python3-venv \
    && pip3 install poetry wheel \
    && pip3 install --upgrade setuptools pip

WORKDIR /service-template

COPY service service/
COPY poetry.lock pyproject.toml /service-template/

RUN poetry install --no-dev \
    && poetry shell \
    && pip3 wheel --wheel-dir=./wheels .


FROM base-image:latest AS runtime-image

ENV APP_HOST=0.0.0.0
ENV APP_PORT=8000

COPY --from=build-image /service-template/wheels /wheels

RUN pip3 install wheels/* --no-cache-dir \
    && rm -rf wheels

USER app_user

CMD ["python3", "-m", "service.app"]
