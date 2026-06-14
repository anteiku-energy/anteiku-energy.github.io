---
title: "the long way home"
authors: ["kemuri"]
date: 2026-06-14
summary: "Remote port forwarding: empurrando a shell de volta pra casa."
tags: ["networking", "pivoting", "port-forwarding"]
draft: false
unlisted: true
---

<figure style="text-align: center;">
  <img
    src="/images/pivoting/4.png"
    alt="A character from Yuumei's Fisheye Placebo facing a glowing screen in a neon city."
    loading="lazy"
    width="799"
    height="664"
    style="max-width: 88%; height: auto;"
  />
  <figcaption>
    &ldquo;Fisheye Placebo&rdquo; art by
    <a href="https://www.yuumeiart.com" target="_blank" rel="noopener">Yuumei</a>.
  </figcaption>
</figure>

- Ao invez de abrir um listener local e encaminhar um servidor no host remoto para a nossa porta, o `Remote Port Forwarding` encaminha um `servico local` para um `listener remoto`. Vamos supor que nossa `Attack Host` tem acesso a um `Ubuntu Victim`. Vamos supor que hipoteticamente pegamos as credenciais de um usuario de uma maquina `Windows` porem a maquina `Windows` consegue se conectar apenas para a subnet `172.16.0.0/17`.

<img
  class="shot"
  src="/images/pivoting/1.png"
  alt="Topologia: Attack Host com acesso ao Ubuntu Victim, e o Windows Victim restrito a subnet 172.16.0.0/17."
  loading="lazy"
/>

https://www.calculator.net/ip-subnet-calculator.html?cclass=any&csubnet=17&cip=172.16.5.0&ctype=ipv4&x=Calculate

- Como voce podem ver o Ubuntu faz parte da subnet que o windows consegue estabelecer conexoes
    - IP RANGE: `172.16.0.1 - 172.16.127.254`.
- Nosso `Pivot Host` sera o `Ubuntu`, visto que ele consegue se comunicar com ambas maquinas (`Attack Host` e `Windows Victim`)

### Attack Chain

- Com todo contexto a cima, nossa chain para obter uma shell na maquina `Windows` seria:
    - Configurar um payload onde o `LOCAL HOST` seria o IP do `Ubuntu (172.16.5.121)` e a `LOCAL PORT` seria a 8080.
    - A porta 8080 serviria para encaminhar todos os pacotes da shell do Windows para a porta 8000 do `Attack Host`.
    - Isso sera possivel configurando um `Remote Port Forward` da maquina `Ubuntu` (na porta 8080) para todas interfaces de rede do `Attack Host` na porta 8000.

### Estabelecendo Conexao e enviando Payload

<img
  class="shot"
  src="/images/pivoting/2.png"
  alt="Estabelecendo a conexao e enviando o payload a partir do Pivot Host."
  loading="lazy"
/>

### Fluxo final entre Windows -> Ubuntu -> Attack Host

<img
  class="shot"
  src="/images/pivoting/3.png"
  alt="Fluxo final do trafego: Windows para Ubuntu para Attack Host."
  loading="lazy"
/>
