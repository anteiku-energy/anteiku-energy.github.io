---
title: "bending traffic to my will"
authors: ["kemuri"]
date: 2026-06-13
summary: "Port forwarding pra pivoting, do meu jeito."
tags: ["networking", "pivoting", "port-forwarding"]
draft: false
unlisted: true
---

<figure>
  <img
    src="/images/city-of-mine-yuumei.jpg"
    alt="A neon cityscape at night, by Yuumei."
    loading="lazy"
    width="1500"
    height="894"
  />
  <figcaption>
    &ldquo;City of Mine&rdquo; art by
    <a href="https://www.yuumeiart.com" target="_blank" rel="noopener">Yuumei</a>.
  </figcaption>
</figure>

- Agora que refrescamos a mente com certos fundamentos de rede por tras do pivoting, iremos abordar uma das tecnicas mais usadas na pratica.

### Port Forwarding

- O `Port Forwarding` eh uma tecnica que consiste em redirecionar uma solicitacao de comunicacao de uma rede para a outra, associando um endereco IP e numero de porta a outro. Na pratica, o invasor faz com que o trafego enviado para uma porta especifica na maquina atacante seja encaminhado atraves da maquina comprometida, eh uma tecnica muito usada para acessar servicos internos.

### Dynamic Port Forwarding

https://github.com/haad/proxychains

- Supondo que nao sabemos qual porta a gente pode acessar e nosso objetivo eh fazer um scan uma subnet, nao sera possivel fazer diretamente do host do atacante, pois nao possuimos rota para a subnet. Para fazer isso teremos que realizar o `Dynamic Port Forwarding`, podemos fazer isso rodando o SSH da nossa maquina com `-D` para abrir um `listener SOCKS` na nossa localhost, e o trafego sai pelo host comprometido que tem acesso a subnet alvo.
    - `ssh -N -f -D 9050 victim@10.129.202.64`
    - Para informar o `proxychains` que precisamos usar a porta `9050` (porta que colocamos no listener SOCKS) a gente edita o arquivo `/etc/proxychains.conf` e adiciona `socks4 127.0.0.1 9050` na ultima linha do arquivo.
    - Proxy SOCKS eh um protocolo de internet que intermedia conexoes entre um cliente (voce ou um aplicativo) a um servidor.

OBS: A flag `-D` faz o tunel SSH transportar apenas conexoes TCP.

OBS 2: SOCKS4 eh limitado APENAS para TCP, suporte a receber pacotes UDP chegou no SOCKS5.
