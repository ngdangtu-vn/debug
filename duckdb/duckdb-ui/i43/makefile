.PHONY: run ls.image ls.container ls.network ls.volume

run:
	@BUILDKIT_PROGRESS=plain docker compose build
	@docker compose up
	@docker compose down --volumes --remove-orphans --rmi "all"

ls.image:
	@docker images

ls.container:
	@docker container ls -a

ls.network:
	@docker network ls

ls.volume:
	@docker volume ls