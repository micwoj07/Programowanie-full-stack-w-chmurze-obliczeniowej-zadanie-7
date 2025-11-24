# Programowanie-full-stack-w-chmurze-obliczeniowej-zadanie-7

###############################################################
LABORATORIUM 7
Autor: Michał Wójtowicz
Grupa: 2.4
###############################################################


Instrukcja uruchomienia

--Utworzenie przestrzeni nazw remote--

kubectl apply -f namespace-remote.yaml
kubectl get ns

--Uruchomienie serwera WWW w przestrzeni remote--

kubectl apply -f remoteweb-pod.yaml
kubectl apply -f remoteweb-service.yaml

--Sprawdzenie działania:--

kubectl get pods -n remote
kubectl get svc -n remote

--Uruchomienie testowego Poda BusyBox--

kubectl apply -f testpod.yaml
kubectl get pods

--Test połączenia z remoteweb z wnętrza testpod--

--Wejście do poda:--
kubectl exec -it testpod -- sh

--Test dostępności:--
wget --spider --timeout=1 http://remoteweb.remote.svc.cluster.local/

--Dostęp do strony przez Minikube--

minikube ip
curl http://$(minikube ip):31999




Krótki opis rozwiązania:


W ramach zadania została utworzona osobna przestrzeń nazw remote, w której uruchomiono Poda remoteweb opartego na obrazie Nginx. Pod został wystawiony na zewnątrz klastra za pomocą obiektu typu NodePort, co umożliwiło dostęp do strony Nginx przez port 31999 na węźle Minikube.

W przestrzeni domyślnej uruchomiono dodatkowo Poda testpod opartego na obrazie BusyBox z poleceniem sleep infinity, aby móc wykonać testy łączności. Z jego poziomu sprawdzono, czy aplikacja webowa działa oraz czy możliwe jest nawiązanie połączenia wewnątrz klastra poprzez DNS Kubernetes.

Ostatecznie zweryfikowano również dostępność strony z poziomu hosta poprzez Minikube, potwierdzając prawidłowe działanie konfiguracji sieciowej, Poda oraz serwisu NodePort.

