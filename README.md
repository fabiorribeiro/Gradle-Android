# Gradle Android
## Para que serve e como utilizar o Gradle para Android

### O que é o Gradle
O Gradle é uma ferramenta ??de gerenciamento?? de compilação ...

### Gradle no Android
No caso do Android, no Gradle são definidos várias coisas, como:
* Para quais dispositivos o aplicativo pode ser executado
* Compilar todo código e recursos para um executável
* Declarar e gerenciar dependências de blibliotecas
* Assinar o aplicativo, necessário para publicar o aplicativos na Google Play Store
* Executar testes automatizados
* etc

"
O sistema de compilação do Android compila os recursos e o código-fonte do app e os empacota em APKs ou Android App Bundles que você pode testar, implantar, assinar e distribuir. O Android Studio usa o Gradle (link em inglês), um kit de ferramentas de compilação avançado, para automatizar e gerenciar o processo de compilação, permitindo que você defina configurações de build personalizadas e flexíveis. Cada configuração de build pode definir o próprio conjunto de códigos e recursos, reutilizando as partes comuns a todas as versões do app. O plug-in do Android para Gradle trabalha com o kit de ferramentas de compilação para fornecer processos e configurações ajustáveis que são específicas para criar e testar apps Android.
"


Ao executar o projeto, o Gradle executa uma série de comandos que compila todo o código-fonte, dependências e qualquer recurso associado ao app, em um aplicativo Android, APK ou ??App Bundle??

No Android existem pelo menos dois arquivos Gradle:
* **build.gradle (Project:{nome do projeto})** - Usado para configurações de compilação do projeto como um todo
* **build.gradle (Module:{nome do módulo})** - Usado para configurações de compilação do módulo específico.

### Estrutura de um arquivo Gradle de projeto: "build.gradle (Project:)"

Este exemplo é de um arquivo de um projeto Android sem nenhuma alteração, criado automaticamente pelo Android Studio.
```Groovy

// O bloco buildscript é onde você configura os repositórios e dependências do próprio Gradle.
// Por exemplo, este bloco inclui o plug-in Android para Gradle como uma dependência porque fornece
// as instruções adicionais de que o Gradle precisa para construir módulos de aplicativos Android.
// Você não deve incluir dependências para seus módulos aqui.
buildscript {

    // O bloco de repositórios configura os repositórios que o Gradle usa para pesquisar ou baixar
    // as dependências para todo o projeto. Você também pode usar repositórios locais ou definir
    // seus próprios repositórios remotos.
    // O código a seguir define  que o Gradle deve usar os repositórios google() e mavenCentral()
    // para procurar suas dependências.
    repositories { 
        google()
        mavenCentral()
    }

    // O bloco de dependências configura as dependências que o Gradle precisa usar para criar seu projeto.
    // Como o Gradle é uma ferramente genérica para compilação, ele sozinho não sabe processar arquivos Kotlin
    // nem criar aplicativos para Android, por isso essas dependências são assenciais, pois informam quais os scripts
    // serão baixados para o Gradle conseguir compilar aplicativos para Android, no nosso caso, em Kotlin. Esses scripts
    // fazem parte dos plug-ins do Gradle que são informadas quais devem ser baixadas no código abaixo.
    // O código a seguir adiciona os plug-ins do Android e Kotlin para Gradle.
    dependencies {
        classpath "com.android.tools.build:gradle:7.0.1"
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:1.4.32"
    }
}

// O bloco allprojects é onde você configura os repositórios e dependências usados por todos os módulos em seu projeto,
// como plug-ins ou bibliotecas de terceiros. No entanto, você deve configurar dependências específicas do módulo
// em cada arquivo build.gradle de nível de módulo.
allprojects { 
    repositories { 

    }

    dependencies {

    }
}

// DESCOBRIR PARA QUE SERVE ISSO
task clean(type: Delete) {
    delete rootProject.buildDir
}
```
*Obs.: É necessário conciliar a versão do plug-in do Adnroid com a [versão do Gradle](#atualizar-a-versão-do-gradle). As versões correspondentes entre
um e outro podem ser enontrados neste link: https://developer.android.com/studio/releases/gradle-plugin?hl=pt-br*


### Estrutura de um arquivo Gradle de um módulo: "build.gradle (Module:)"

Este exemplo é de um arquivo de um projeto Android sem nenhuma alteração, criado automaticamente pelo Android Studio.
```Groovy
// Neste bloco são especificados quais os plugins serão aplicados para este módulo específico, para informar ao Gradle,
// no nosso caso, como processar arquivos Kotlin e gerar aplicativos para Android.
// Esses plug-ins são baixados das dependências informadas no arquivos Gradle do projeto (na seção anterior)
// Neste caso, é aplicado o plugin de uma aplicação Android e do Kotlin para Android
plugins {
    id 'com.android.application'
    id 'kotlin-android'
}

// Neste bloco, são definidas todas as especificações para compilar o projeto
android {

    // A versão da API em que o aplicativo é realmente compilado
    compileSdk 30

    defaultConfig {
        
        // 'applicationId' é o identificador exclusivo que o Android e o Google Play usam para identificar seu aplicativo.
        // Por exemplo, não é possivel instalar dois aplicativos diferentes em um celuar com o mesmo applicationId,
        // nem publicar na Google Play um aplicativo com um ID que já foi utilizado por outro aplicativo.
        // Portanto é muito importante que ele seja único. Por isso é comum usarem o nome reverso do domínio
        // das empresas para definir o ID do aplicativo. A lógica é que apenas uma empresa ou pessoa terá
        //realmente a propriedade daquele domínio na web.
        applicationId "com.exemple.tutotialgradle"
        
        // Versão mínima que o aplicativo suporta.
        minSdk 20
        
        // Target informa em quais versões seu aplicativo foi testado. Se o 'minSdk' é 20 e o 'targetSdk' 30,
        // significa que você testou seu aplicaivos das versões de 20 até 30
        targetSdk 30
        
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    kotlinOptions {
        jvmTarget = '1.8'
    }
}

// O bloco de dependências no arquivo Gradle do módulo especifica as dependências necessárias para construir
// apenas este módulo. Elas são baixadas dos repositórios informados no arquivo Gradle do projeto.
dependencies {
    implementation "androidx.core:core-ktx:1.6.0"
    implementation 'androidx.appcompat:appcompat:1.3.1'
    implementation 'com.google.android.material:material:1.4.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.1'
    testImplementation 'junit:junit:4.+'
    androidTestImplementation 'androidx.test.ext:junit:1.1.3'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.4.0'
}
```

# Atualizar a versão do Gradle

### Outros blocos interessantes que podem ser usados em um arquivo Gradle

```Groovy
// Neste bloco, você pode criar variáveis que serão usadas nos arquivos Gradle.
// No código abaixo, foi criada uma variál com a verão do plugin do kotlin
ext { 
    kotlin_version = "1.4.32"
}
```
Fontes:
https://developer.android.com/studio/build#kts
https://sodocumentation.net/android-gradle/topic/2161/configure-your-build-with-gradle
