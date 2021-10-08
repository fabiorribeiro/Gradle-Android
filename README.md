# Gradle Android
## Para que serve e como utilizar o Gradle para Android

### O que é o Gradle
O Gradle é uma ferramenta ??de gerenciamento?? de compilação ...

### Gradle no Android
No caso do Android, no Gradle são definidos várias coisas, como:
* Para quais dispositivos o aplicativo pode ser executado
* Compilar todo código e recursos para um executável
* Declarar e gerenciar dependências de blibliotecas
* ??Assinar o aplicativo??
* Executar testes automatizados
* etc

"
O sistema de compilação do Android compila os recursos e o código-fonte do app e os empacota em APKs ou Android App Bundles que você pode testar, implantar, assinar e distribuir. O Android Studio usa o Gradle (link em inglês), um kit de ferramentas de compilação avançado, para automatizar e gerenciar o processo de compilação, permitindo que você defina configurações de build personalizadas e flexíveis. Cada configuração de build pode definir o próprio conjunto de códigos e recursos, reutilizando as partes comuns a todas as versões do app. O plug-in do Android para Gradle trabalha com o kit de ferramentas de compilação para fornecer processos e configurações ajustáveis que são específicas para criar e testar apps Android.
"


Ao executar o projeto, o Gradle executa uma série de comandos que compila todo o código-fonte, dependências e qualquer recurso associado ao app, em um aplicativo Android, APK ou ??App Bundle??

No Android existem pelo menos dois arquivos Gradle:
* **build.gradle (Project:{nome do projeto})** - Usado para configurações de compilação do projeto como um todo
* **build.gradle (Module:{nome do módulo})** - Usado para configurações de compilação do módulo específico.

### Arquivo Gradle do projeto: build.gradle (Project:)
Estrutura do arquivo

* buildscript { }

O bloco buildscript é onde você configura os repositórios e dependências do próprio Gradle - ou seja, você não deve incluir dependências para seus módulos aqui. Por exemplo, este bloco inclui o plug-in Android para Gradle como uma dependência porque fornece as instruções adicionais de que o Gradle * precisa para construir módulos de aplicativos Android.

* ext { }

Bloco para criar variáveis que será usadas no Gradle

* repositories { }

O bloco de repositórios configura os repositórios que o Gradle usa para pesquisar ou baixar as dependências. O Gradle pré-configura o suporte para repositórios remotos, como JCenter, Maven Central e Ivy. Você também pode usar repositórios locais ou definir seus próprios repositórios remotos. O código a seguir define JCenter como o repositório que o Gradle deve usar para procurar suas dependências.


* dependencies { }

O bloco de dependências configura as dependências que o Gradle precisa usar para criar seu projeto. A linha a seguir adiciona o plug-in do Android para Gradle versão 4.2.0 como uma dependência do classpath.

* allprojects { }

O bloco allprojects é onde você configura os repositórios e dependências usados por todos os módulos em seu projeto, como plug-ins ou bibliotecas de terceiros. No entanto, você deve configurar dependências específicas do módulo em cada arquivo build.gradle de nível de módulo. Para novos projetos, o Android Studio inclui o JCenter e o repositório Maven do Google por padrão, mas não configura nenhuma dependência (a menos que você selecione um modelo que exija algumas).

### Arquivo Gradle do múdulo: build.gradle (Module:)

* plugins { }

A primeira seção na configuração da compilação aplica o plugin Android para * Gradle para esta compilação e torna o bloco android disponível para especificar * Opções de construção específicas para Android.

<span style="color:blue">_O bloco android é onde você configura todas as opções de construção específicas do Android._</span><br>
android { 




}


* compileSdkVersion(xx) { }

* buildToolsVersion(xx) { }

* defaultConfig { }

* { }

* { }


* dependencies { }

O bloco de dependências no arquivo de configuração de construção de nível de módulo * especifica as dependências necessárias para construir apenas o próprio módulo.

Fontes:
https://developer.android.com/studio/build#kts
