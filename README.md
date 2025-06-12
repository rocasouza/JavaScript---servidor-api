# EasyCar API Documentation

## Visão Geral

A API EasyCar gerencia o processo de solicitação e atribuição de caronas entre passageiros e motoristas. Ela permite que usuários solicitem caronas, motoristas aceitem e gerenciem essas caronas, e que o sistema rastreie o status de cada corrida.

Rotas - Projeto EasyCar
----------------------------------------

### Recursos Principais

#### Corridas (`/rides`)

**Consultar Corridas**
*   `GET /rides`
    *   Descrição: Retorna uma lista de corridas. Pode ser filtrada para verificar corridas pendentes para o usuário logado ou por outros critérios.
    *   Filtros (Query Params):
        *   `passenger_user_id`: ID do usuário passageiro.
        *   `pickup_date`: Data da corrida (para consultar corridas de hoje).
        *   `ride_id`: ID específico da corrida.
        *   `driver_user_id`: ID do usuário motorista.
        *   `status`: Status da corrida (ex: `pending`, `accepted`, `finished`, `canceled`).
    *   Exemplo de Resposta (Sucesso 200):
        ```json
        [
          {
            "ride_id": "123",
            "passenger_user_id": "user_abc",
            "driver_user_id": null,
            "pickup_location": "Rua Exemplo, 123",
            "dropoff_location": "Avenida Teste, 456",
            "pickup_date": "2024-08-15T10:00:00Z",
            "status": "pending"
          }
        ]
        ```

**Cadastrar Nova Carona**
*   `POST /rides`
    *   Descrição: Cria um novo pedido de carona.
    *   Corpo da Requisição (JSON):
        ```json
        {
          "passenger_user_id": "user_abc",
          "pickup_location": "Rua Origem, 10",
          "dropoff_location": "Rua Destino, 20",
          "pickup_date": "2024-08-15T14:30:00Z"
        }
        ```
    *   Resposta (Sucesso 201 - Created): Retorna os dados da corrida criada.
    *   Resposta (Erro 400 - Bad Request): Se o usuário já tiver uma carona pendente.

**Consultar Dados de uma Corrida Específica**
*   `GET /rides/{ride_id}`
    *   Descrição: Retorna todos os dados de uma determinada corrida.
    *   Parâmetros de Caminho:
        *   `ride_id`: ID da corrida.
    *   Resposta (Sucesso 200): Retorna os detalhes da corrida.
    *   Resposta (Erro 404 - Not Found): Se a corrida não existir.

**Cancelar Pedido de Carona (Passageiro)**
*   `DELETE /rides/{ride_id}`
    *   Descrição: Permite que o passageiro cancele um pedido de carona.
    *   Parâmetros de Caminho:
        *   `ride_id`: ID da corrida a ser cancelada.
    *   Resposta (Sucesso 204 - No Content): Corrida cancelada com sucesso.
    *   Resposta (Erro 403 - Forbidden): Se o usuário não for o passageiro da corrida ou se a corrida não puder ser cancelada (ex: já iniciada).

**Finalizar Corrida (Motorista)**
*   `PUT /rides/{ride_id}/finish`
    *   Descrição: Marca uma carona como concluída pelo motorista.
    *   Parâmetros de Caminho:
        *   `ride_id`: ID da corrida.
    *   Corpo da Requisição (JSON):
        ```json
        {
          "passenger_user_id": "user_abc" // Confirmação do passageiro
        }
        ```
    *   Resposta (Sucesso 200): Corrida finalizada com sucesso.

**Motorista Aceitar Carona**
*   `PUT /rides/{ride_id}/accept`
    *   Descrição: Permite que um motorista aceite uma carona pendente.
    *   Parâmetros de Caminho:
        *   `ride_id`: ID da corrida.
    *   Corpo da Requisição (JSON):
        ```json
        {
          "driver_user_id": "driver_xyz"
        }
        ```
    *   Resposta (Sucesso 200): Carona aceita.
    *   Resposta (Erro 400 - Bad Request): Se o motorista já tiver uma carona ativa ou se a corrida não estiver disponível.

**Motorista Cancelar Carona Aceita**
*   `PUT /rides/{ride_id}/cancel`
    *   Descrição: Permite que um motorista cancele uma carona que ele aceitou. A corrida volta ao status `pending`.
    *   Parâmetros de Caminho:
        *   `ride_id`: ID da corrida.
    *   Resposta (Sucesso 200): Carona cancelada pelo motorista.

#### Corridas para Motoristas (`/rides/drivers`)

**Consultar Corridas para Motorista**
*   `GET /rides/drivers/{driver_user_id}`
    *   Descrição: Consulta as corridas atribuídas ao motorista e as corridas pendentes que ele pode aceitar.
    *   Parâmetros de Caminho:
        *   `driver_user_id`: ID do motorista.
    *   Resposta (Sucesso 200): Lista de corridas.

Business Rules (Regras de Negocio)
----------------------------------------

*   **Uma Carona por Vez (Passageiro):** O usuário (passageiro) só pode ter uma carona com status `pending` ou `accepted` por vez.
*   **Uma Carona por Vez (Motorista):** O motorista só pode ter uma carona com status `accepted` por vez.
*   **Cancelamento:**
    *   Passageiros podem cancelar corridas `pending` ou `accepted` (antes do início efetivo).
    *   Motoristas podem cancelar corridas `accepted`, retornando-as ao status `pending` para que outro motorista possa aceitar.
*   **Finalização:** Apenas o motorista atribuído pode finalizar uma corrida.

## Status das Corridas

*   `P`: Corrida solicitada, aguardando motorista.
*   `A`: Motorista aceitou a corrida.
*   `in_progress`: Corrida em andamento (opcional, se houver rastreamento em tempo real). *Não implementado*
*
*   `F`: Corrida concluída com sucesso.
*   `canceled_by_passenger`: Corrida cancelada pelo passageiro. *Não implementado*
*   `canceled_by_driver`: Corrida cancelada pelo motorista (após aceitar). *Não implementado*

## Considerações Futuras (Exemplos)

*   Autenticação e Autorização (ex: JWT).
*   Paginação para listas longas.
*   Validação de entrada mais detalhada.
*   Tratamento de erros e códigos de status HTTP mais específicos.
*   Notificações (ex: para motoristas sobre novas corridas, para passageiros sobre status).
