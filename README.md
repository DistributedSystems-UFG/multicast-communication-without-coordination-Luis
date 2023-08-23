# Lab. Sistemas Distribuídos 02/08/23 - Multicast Communication Without Coordination

# Introdução:

O multicast é um método de comunicação em redes que permite a transmissão eficiente de dados para múltiplos destinatários simultaneamente (ao contrário da comunicação unicast, em que cada mensagem é enviada individualmente para cada destinatário). O multicast permite que um único pacote seja enviado para um grupo de receptores interessados, isso otimiza a largura de banda da rede, pois os dados são replicados somente quando necessário, alcançando todos os membros do grupo de uma só vez.

# Demonstração simples de comunicação multicast sem coordenação.

### comparisonServer.py:

- O servidor é criado em um socket e fica ouvindo por conexões em um endereço e porta definidos nas constantes.
- Ele espera receber um número específico de listas de mensagens dos nós participantes.
- Após receber todas as listas de mensagens, ele compara as mensagens em cada rodada para verificar se estão na mesma ordem.
- Ele imprime o número de rodadas de mensagens que estão fora de ordem.

### peerCommunicatorUDP.py

Esse é o comunicador de pares que coordena o envio e recebimento de mensagens entre os nós participantes.

- Ele cria um socket UDP para receber e enviar mensagens.
- Um thread separado (MsgHandler) é criado para lidar com as mensagens recebidas.
- Ele envia mensagens de "handshake" para todos os outros nós para indicar que está pronto para enviar e receber mensagens.
- Quando recebe confirmações de "handshake" de todos os nós, ele começa a enviar uma sequência de mensagens para os outros nós.
- Após enviar todas as mensagens, ele envia uma mensagem de "stop" para todos os outros nós para indicar que não tem mais mensagens para enviar.
- As mensagens são enviadas ao servidor de comparação para verificar a ordem.

# Conclusão e Problema de Coordenação:

O sistema implementado realiza a comunicação multicast entre vários nós (n° declarado juntamente com as informações de comunicação presentes em const.py), cada nó envia mensagens aos outros nós e recebe mensagens deles, porém o problema de coordenação surge na comparação das mensagens para verificar a ordem correta, algumas alternativas para contornar o problema de coordenação e melhorar a confiabilidade da comunicação multicast, podemos considerar o uso das seguintes medidas:

- Protocolo de Ordenação: Podemos implementar um protocolo de ordenação de mensagens, como "Total Order Broadcast", para garantir que todas as mensagens sejam entregues em ordem em todos os nós participantes.
- Garantir Entrega Confiável: Podemos usar também um protocolo de entrega confiável de mensagens, como ACK/NACK, para garantir que as mensagens sejam recebidas por todos os nós e confirmar a entrega.
- Coordenação Centralizada: Podemos introduzir um servidor centralizado que gerencie a coordenação entre os nós, incluindo a garantia da ordem das mensagens.
- Mecanismo de Confirmação: E por fim, podemos implementar um mecanismo de confirmação para garantir que as mensagens sejam recebidas e processadas corretamente antes de avançar para a próxima rodada.
