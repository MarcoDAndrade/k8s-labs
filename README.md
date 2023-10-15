# k8s-labs

Experimentos em K8s e fontes de consulta

## Montando um ambiente

Apesar de muita referencia falar sobre o Minikube, eu já testei no passado e confesso que não gostei de colocar dentro do meu windows... Não reconhecer os elementos e os impactos, deixa a sensação de bagunça e falta de controle.

Afinal, em tempos de foco na cyber-segurança, fazer o que você não entende no teu computador, que acessa o banco, faz imposto de renda, salva teus dados importantes? é melhor deixar pra outra hora ;)

Para resolver este impasse, localizei um material excelente, que te entrega o [laboratorio perfeito](https://github.com/techiescamp/vagrant-kubeadm-kubernetes), através de imagens VirtualBox (no meu caso), e o melhor, com um ótimo código em [Vagrant](https://www.vagrantup.com/) para você ajustar para suas necessidades, que é claro eu fiz ;)


### Mão na massa

Aqui os meus passos basicos:

- Instalei um [Virtualbox](https://www.virtualbox.org/wiki/Downloads) basicão para segregar meu ambiente;
- Instalei o [Vagrant](https://developer.hashicorp.com/vagrant/downloads?product_intent=vagrant);
- Clonei o repositorio [vagrant-kubeadm-kubernetes](https://github.com/techiescamp/vagrant-kubeadm-kubernetes) e extrai os dados relevantes:

    - Vagrantfile - Arquivo que define a configuração do ambiente;
    - settings.yaml - Arquivo com a parametrização do ambiente;

Não vou discorrer sobre o resultado, para isto na pasta [lab][./lab]!

Mas e agora, o que eu faço?

Eu investi a alguns anos, mas não exercitei, então estou recomeçando, e gostei muito do material dispnibilizado pela [LinuxTips](https://www.linuxtips.io/treinamentos), começando pela trilha Essentials:

- Treinamento gratuito de [Kubernetes Essentials](https://www.linuxtips.io/course/kubernetes-essentials)!
- [Livro Descomplicando Kubernetes](https://livro.descomplicandokubernetes.com.br/pt/)

Nos meus planos, o proximo passo (em Kubernetes), vai ser investir no treinamento completo!
O intensivão está em andamento a 3 meses, então vou ter tempo de priorizar as outras tecnologias ;)


### Sobre o Vagrant

Trata-se de uma ferramenta sensacional para criar laboratorios, onde você prepara tua infraestrutura de laboratorio por IaC ! Isso mesmo... resumindo... "quase um Terraform", que te entrega Infra as Code! Ou podemos chamar de LaC, para Lab as Code! rs

[Vagrant Up](https://www.vagrantup.com/) - site onde vai achar as imagens e material de aprendizado!

