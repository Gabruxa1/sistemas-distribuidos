Como Executar o Programa de Prioridade de Threads em Java
=========================================================

Este tutorial guia você na criação de um programa em Java que demonstra a execução de threads com diferentes níveis de prioridade. O programa cria uma thread de baixa prioridade e uma de alta prioridade para ilustrar a diferença na execução das threads. Siga os passos abaixo para configurar e executar o programa no Linux.

* * * * *

Passo 1: Criar o Arquivo Java
-----------------------------

Crie um novo arquivo Java em sua pasta de trabalho. Dê um nome ao arquivo, por exemplo, `LancadorPrioridade.java`.

Passo 2: Adicionar o Código
---------------------------

Adicione o seguinte trecho de código ao seu arquivo Java. Ele inclui a definição das threads de baixa e alta prioridade e o código para gerenciá-las.

```java
class BaixaPrioridade extends Thread {
    public void run() {
        setPriority(Thread.MIN_PRIORITY);
        for(;;) {
            System.out.println("Thread de baixa prioridade executando -> 1");
        }
    }
}

class AltaPrioridade extends Thread {
    public void run() {
        setPriority(Thread.MAX_PRIORITY);
        for(;;) {
            for(int i=0; i<5; i++)
                System.out.println("Thread de alta prioridade executando -> 10");
                try {
                    sleep(10);
                } catch(InterruptedException e) {
                    System.exit(0);
                }
        }
    }
}

class LancadorPrioridade {
    public static void main(String args[]) {
        AltaPrioridade a = new AltaPrioridade();
        BaixaPrioridade b = new BaixaPrioridade();
        System.out.println("Iniciando threads...");
        b.start();
        a.start();
        Thread.currentThread().yield();
        System.out.println("Lancador Prioridade finalizado.");

        Runtime.getRuntime().addShutdownHook(new Thread(() -> {
            System.out.println("\nEncerrando threads Concorrente com ShutdownHook...");
            a.interrupt();
            b.interrupt();
        }));

        try {
            Thread.sleep(50000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        a.interrupt();
        b.interrupt();
        System.out.println("Finalizando Threads...");
    }
}
````
### Explicação do Código

-   **BaixaPrioridade**: Uma thread que executa em baixa prioridade, imprimindo uma mensagem continuamente.
-   **AltaPrioridade**: Uma thread de alta prioridade que imprime uma mensagem em loop com pequenas pausas.
-   **LancadorPrioridade**: A classe principal que inicializa as duas threads e usa um `ShutdownHook` para interromper as threads quando o programa é finalizado.

Passo 3: Compilar o Arquivo Java
--------------------------------

Abra o terminal, navegue até a pasta onde o arquivo `.java` foi salvo e execute o comando abaixo para compilar o programa:

`javac LancadorPrioridade.java`

Esse comando cria um arquivo `.class` com o código compilado, necessário para executar o programa.

Passo 4: Executar o Programa
----------------------------

Ainda no terminal, execute o programa com o comando:

`java LancadorPrioridade`

### O que Esperar

-   O programa inicia as duas threads com prioridades distintas.
-   A thread de alta prioridade imprime mensagens com mais frequência, enquanto a de baixa prioridade executa de forma mais lenta.
-   Ao final, o `ShutdownHook` garante que as threads sejam interrompidas de forma segura.
