
protos:
	python -m grpc_tools.protoc ./nucliadb_telemetry/tests/grpc/helloworld.proto           -I ./ --python_out=./ --mypy_out=./ --grpc_python_out=./ --mypy_grpc_out=./
	python -m grpc_tools.protoc ./nucliadb_telemetry/tests/grpc/hellostreamingworld.proto  -I ./ --python_out=./ --mypy_out=./ --grpc_python_out=./ --mypy_grpc_out=./

.PHONY: install-dev
install-dev:
## TODO: remove pip version install when pip is fixed. Right now it was making mypy fail.
	pip install --upgrade "pip<23.1"
	pip install -r ../test-requirements.txt
	pip install -r ../code-requirements.txt
	pip install -r requirements.txt
# utils dep required for test fixtures for nats
# NEEDS TO BE DECOUPLED EVENTUALLY
	pip install -e ../nucliadb_protos/python
	pip install -e ../nucliadb_utils
	pip install -e .

.PHONY: format
format:
	isort --profile black .
	black .

.PHONY: lint
lint:
	flake8 nucliadb_telemetry --config=setup.cfg
	isort -c --profile black .
	black --check .
	MYPYPATH=../mypy_stubs mypy --config-file=../mypy.ini .

.PHONY: test
test:
	pytest -s --tb=native -v nucliadb_telemetry

.PHONY: test-cov
test-cov:
	pytest -rfE --cov=nucliadb_telemetry --cov-config=../.coveragerc -s --tb=native -v --cov-report term-missing:skip-covered --cov-report xml nucliadb_telemetry
