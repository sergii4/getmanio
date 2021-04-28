---
title: "Kubens. DIY with FZF"
date: 2021-04-28T15:56:07+01:00
tags: ["kubernetes", "k8s", "fzf", "fish", "shell"]
keywords: ["kubernetes", "k8s", "fzf", "fish", "shell"]
images:
    - /img/no-headers.png
draft: false 
---
![yamaha](/img/yamaha.jpg)


In my work daily routine, I often need to switch between namespaces(less frequently contexts) in kubernetes and I used to use [kubectx/kubens](https://github.com/ahmetb/kubectx) tools. Recently I started using [FZF](https://github.com/junegunn/fzf), mostly as a file finder and for fuzzy search in command line history.

I was wonder if I can apply FZF somewhere else. I tried to make my own kubens. What requirements do I have?
- show all namespaces for the current context
- select with fuzzy search
- switch to selected
- highlight current namespace (optional) 

For intermediate scripts I use **fish shell**, a final script I show in fish shell and **bash**/**zsh**.

## Show namespaces
Default kubectl command:
```
kubectl get namespace
```

## Select with fuzzy search
To select namespace we pipe previous command with FZF:
```
kubectl get namespace | fzf
```

![with headers](/img/with-headers.png)

We have to get rid of headers and choose only namespace name column:
```
kubectl get namespace --no-headers | fzf | awk '{ print $1}'
```

![no headers](/img/no-headers.png)

## Swith to selected
```
kubectl config set-context --current \
	--namespace=(kubectl get namespace \
	--no-headers | fzf | awk '{ print $1}')
```

bash/zsh:

```
kubectl config set-context --current \ 
	--namespace=$(kubectl get namespace \
	--no-headers | fzf | awk '{ print $1}') 
```
[![asciicast](https://asciinema.org/a/410471.svg)](https://asciinema.org/a/410471)

## Conclusion

We can add it to fish functions and invoke by name(`knmps`).

Add _knmsp.fish_ to ` ~/.config/fish/functions/knmsp.fish`
```
function knmsp -d "run vim with fzf"
  kubectl config set-context --current \ 
  	--namespace=(kubectl get namespace \
  	--no-headers | fzf | awk '{ print $1}')
end
```
