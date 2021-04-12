# Launch of the Cardano node

Earlier we started the relay in an active ssh session, which means that as soon as we close the browser, the node will stop working.So we need to start it from TMUX.

```text
cd ~/cnode/scripts/
```

Let's create scripts

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

Let's copy them into the Scripts folder.

```text
cp *.sh ~/cnode/scripts/
```

After creating these scripts, you will need to make them executable by using the chmod command

```text
cd ~/cnode/scripts/
chmod +x start_all.sh stop_all.sh node.sh
```

Now we start a node in TMUX.

```text
./start_all.sh
```

