all : init

init :
	sudo docker build -t irc .

on :
	sudo docker run --name irc irc

up :
	sudo docker start irc

down :
	sudo docker stop irc

clean : down
	sudo docker rm irc

fclean :
	sudo docker rmi $$(docker images -a)
	sudo docker system purne -f
