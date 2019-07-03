docker_pull:
	docker pull {{ cookiecutter.name_docker }}:dev
	@echo Fake the tag locally so that the pipeline runs properly
	@echo Looks weird when this is :dev to :dev, but makes sense when testing code for a release (:dev to :1.0.1)
	docker tag {{ cookiecutter.name_docker }}:dev {{ cookiecutter.name_docker }}:dev


docker_build:
	@docker build \
		--build-arg VCS_REF=`git rev-parse --short HEAD`  \
		--build-arg BUILD_DATE=`date -u +"%Y-%m-%dT%H:%M:%SZ"` \
		-t {{ cookiecutter.name_docker }} .


lint: nf_lint md_lint

nf_lint:
	@echo Linting the pipeline code
	nf-core lint .

md_lint:
	@echo Linting the documentation
	markdownlint ${TRAVIS_BUILD_DIR} -c ${TRAVIS_BUILD_DIR}/.github/markdownlint.yml


test:
	nextflow run . -profile test,docker