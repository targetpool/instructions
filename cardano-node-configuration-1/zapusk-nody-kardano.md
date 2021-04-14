# Ausführen eines Cardano Node

Zuvor haben wir das Relais in einer aktiven SSH-Sitzung gestartet, was bedeutet, dass der Knoten nicht mehr funktioniert, sobald wir den Browser schließen.

```text
cd ~/cnode/scripts/
```

Lassen Sie uns Skripte erstellen

{% tabs %}
{% tab title="start\_all.sh" %}
```text
nano start_all.sh
```

> \#!/bin/bash
>
> session="cardano"  
> \# Check if the session exists, discarding output  
> \# We can check $? for the exit status \(zero for success, non-zero for failure\)
>
> tmux has-session -t $session 2&gt;/dev/null  
> if \[ $? != 0 \]; then  
>  tmux attach-session -t $session  
>  tmux new -s "cardano" -n "node" -d  
>  tmux split-window -v  
>  tmux split-window -h  
>  tmux select-pane -t 'cardano:node.0'  
>  tmux split-window -h  
>  tmux send-keys -t 'cardano:node.0' './node.sh' Enter  
>  tmux send-keys -t 'cardano:node.1' 'htop' Enter  
>  tmux send-keys -t 'cardano:node.2' 'nload' Enter  
>  tmux send-keys -t 'cardano:node.3' Enter  
> fi  
>   
> tmux attach-session -t $session
{% endtab %}

{% tab title="stop\_all.sh" %}
```text
nano stop_all.sh
```

> \#!/bin/bash
>
> \# Check if the session exists, discarding output  
> \# We can check $? for the exit status \(zero for success, non-zero for failure\)

> session="cardano"  
> \# Check if the session exists, discarding output  
> \# We can check $? for the exit status \(zero for success, non-zero for failure\)  
>   
> tmux has-session -t $session 2&gt;/dev/null  
> if \[ $? != 0 \]; then  
>         echo "Session not found."  
> else  
>         echo "Killing session"  
>         tmux kill-session -t cardano  
> fi
{% endtab %}

{% tab title="node.sh" %}
```text
nano node.sh
```

> \#!/bin/bash  
> cardano-node run \  
>   --database-path  ~/cnode/db/ \  
>   --socket-path ~/cnode/sockets/node.socket \  
>   --port 3000 \  
>   --config ~/cnode/config/mainnet-config.json \  
>   --topology ~/cnode/config/mainnet-topology.json
{% endtab %}
{% endtabs %}

```text
chmod +x start_all.sh stop_all.sh node.sh
```

Kopieren wir sie in den Ordner "Scripts".

```text
cp *.sh ~/cnode/scripts/
```

Nachdem Sie diese Skripte erstellt haben, müssen Sie sie mit dem Befehl chmod ausführbar machen

```text
cd ~/cnode/scripts/
chmod +x start_all.sh stop_all.sh node.sh
```

Nun starten wir einen Knoten in TMUX.

```text
./start_all.sh
```

