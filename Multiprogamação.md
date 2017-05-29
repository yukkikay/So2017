Programação concorrente

Programação concorrente é um paradigma de programação para a construção de programas de computador que fazem uso da execução concorrente (simultânea) de várias tarefas computacionais interativas, que podem ser implementadas como programas separados ou como um conjunto de threads criadas por um único programa. Essas tarefas podem ser executadas por um único processador, vários processadores em um único equipamento ou processadores distribuídos por uma rede. Programação concorrente é relacionada com programação paralela, mas foca mais na interação entre as tarefas. A interação e a comunicação correta entre as diferentes tarefas, além da coordenação do acesso concorrente aos recursos computacionais são as principais questões discutidas durante o desenvolvimento de sistemas concorrentes. Pioneiros na área de pesquisa incluem Edsger Dijkstra, Per Brinch Hansen, e C.A.R. Hoare.
Vantagens do paradigma incluem o aumento de desempenho, pois aumenta-se a quantidade de tarefas sendo executadas em determinado período de tempo, e a possibilidade de uma melhor modelagem de programas, pois determinados problemas computacionais são concorrentes por natureza


  
  Interação e comunicação concorrente
Em alguns sistemas computacionais concorrentes, a comunicação entre os componentes é escondida do programador, enquanto em outros a comunicação deve ser lida explicitamente. A comunicação explícita pode ser dividida em duas classes: por memória compartilhada ou por troca de mensagens.
Comunicação por memória compartilhada
Componentes concorrentes comunicam-se ao alterar o conteúdo de áreas de memórias compartilhadas, o que geralmente requer o desenvolvimento de alguns métodos de trava como exclusão mútua, semáforo ou monitor para gerenciar a utilização de um determinado recurso entre as tarefas.
Comunicação por troca de mensagens
  
  Componentes concorrentes comunicam-se ao trocar mensagens, cuja leitura pode ser feita assincronamente (também denominada como "enviar e rezar", apesar da prática padrão ser reenviar mensagens que não são sinalizadas como recebidas) ou pelo método rendezvous, no qual o emissor é bloqueado até que a mensagem seja recebida (comunicação síncrona).
A comunicação por mensagens tende a ser mais simples que a comunicação por memória compartilhada, e é considerada uma forma mais robusta de programação concorrente. Uma ampla variedade de teorias matemáticas estão disponíveis para o entendimento e análise de sistemas de comunicação por envio de mensagem, incluindo o modelo de Ator.
Coordenando o acesso aos recursos
Um dos assuntos de maior discussão em programação concorrente é como prevenir que tarefas concorrentes interfiram umas nas outras. Por exemplo, considerando o seguinte algoritmo para realizar saques de uma conta representada pelo recurso compartilhado balanco:
bool saque( int quantia )

{
   if( balanco > quantia )
   {
      balanco = balanco - quantia;
      return true;
   }
   else
   {
      return false;
   }
}
    
    Suponha que balanco = 500, e dois processos concorrentes realizam a chamada saque(300) e saque(350), respectivamente. Se em ambas as operações a linha 3 é executada antes da linha 5 do processo concorrente, ambas as operações irão deduzir que o balanço é maior que a quantia a ser sacada, e a execução irá proceder subtraindo os valores a serem sacados em ambos os processos. Além disso, o resultado final da operação também é indefinido e dependerá da ordem em que os processos terminarem a sua execução, sendo que, o processo que executar a linha 5 por último terá o seu resultado gravado em definitivo. Esse tipo de problema com recursos compartilhados requer o uso de controles de concorrência, ou algoritmos não bloqueantes.
Como sistemas concorrentes necessitam a utilização de recursos compartilhados, a programação concorrente geralmente requer o uso de algum método de árbitro, um elemento neutro, para coordenar o acesso a tais recursos. Isso introduz a possibilidade do aparecimento de problemas com decisões não determinísticas, apesar de que o desenvolvimento cuidadoso de árbitros pode reduzir a probabilidade de tais situações aparecerem.
Este é um exemplo de como resolver o problema da indefinição do resultado da concorrência, acima:
bool saque( int quantia )
{
   pthread_mutex_lock(v_bloqueio);
   if( balanco > quantia )
   {
      balanco = balanco - quantia;
      pthread_mutex_unlock(v_bloqueio);
      return true;
   }
   else
   {
      pthread_mutex_unlock(v_bloqueio);
      return false;
   }
}
  
  
  Nesta alteração, a instrução da linha 3 diz para o processo iniciar o bloqueio marcando na variável "v_bloqueio" que foi ele quem bloqueou. Se esta variável já estiver marcada como "bloqueada", o processo aguarda sua liberação para seguir em frente. Caso contrário, executa as demais instruções. Quando executar o comando para desbloquear, seja na linha 7 ou 12, o próximo processo que estiver aguardando a obtenção do bloqueio irá verificar que pode continuar. Dessa forma, apenas um processo por vez executa o teste e alteração da variável "balanco", com seu valor já atualizado e evitando assim a impresivibilidade do resultado.

    Obviamente, o contratempo desta abordagem é "afunilar" o processamento paralelo. No pior caso, se todos os processos chegarem a este trecho ao mesmo tempo, apenas um de cada vez irá executá-lo. Entretanto, se estas situações forem uma porção mínima em todo o sistema, esta limitação será insignificante, dado a vantagem do restante do processamento paralelo.
Suporte em linguagens
    
    As linguagens de programação concorrente são linguagens de programação que provem construções para a concorrência. Tais construções podem envolver multitarefa, suporte para sistemas distribuídos, troca de mensagens e recursos compartilhados.
Atualmente, as linguagens mais utilizadas para tais construções são Java e C#. Ambas utilizam o modelo de memória compartilhada, com o bloqueio sendo fornecido por monitores. Apesar disso, o modelo de troca de mensagens pode ser implementado sobre o modelo de memória compartilhada. Entre linguagens que utilizam o modelo de troca de mensagens, Erlang é possivelmente a mais utilizada pela indústria atualmente.
  
  Várias linguagens de programação concorrente foram desenvolvidas como objeto de pesquisa, como por exemplo Pict. Apesar disso, linguagens como Erlang, Limbo e Occam tiveram uso industrial em vários momentos desde a década de 1980.
   
  Várias outras linguagens oferecem o suporte à concorrência através de bibliotecas, como por exemplo C e C++.

