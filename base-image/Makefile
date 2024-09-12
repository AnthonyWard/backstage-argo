buildandpush: build push
	echo "finished"
build:
	docker build . -t anthonyward/backstage-base
push: build
	docker push anthonyward/backstage-base