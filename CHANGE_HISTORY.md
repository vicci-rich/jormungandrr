# bds-tron
## src/main/java/org/tron/core/config/args/Args.java
Modify config Args setting to support kafka params.

```
  @Getter
  @Setter
  private String kafkaEndpoint;
  
  INSTANCE.kafkaEndpoint =
              config.hasPath("kafka.endpoint.url") ? config.getString("kafka.endpoint.url") : "";
```

## src/main/java/org/tron/core/services/http/FullNodeHttpApiService.java
Add sendblock and sendbatchblock  methods

```
//send block by num
  @Autowired
  private SendBlockByNumServlet sendBlockByNumServlet;
//send batchblock by num
  @Autowired
  private SendBatchBlockByNumServlet sendBatchBlockByNumServlet;
//send batchblock by id
  @Autowired
  private SendBlockByIdServlet sendBlockByIdServlet;
```

## src/main/java/org/tron/core/services/http/HttpUtil.java
the new file to implement sending data to kafka through http.

## src/main/java/org/tron/core/services/http/SendBatchBlockByNumServlet.java
the new file to implement sending batch block.

## src/main/java/org/tron/core/services/http/SendBlockByIdServlet.java
the new file to implement sending block by id.

## src/main/java/org/tron/core/services/http/SendBlockByNumServlet.java
the new file to implement sending block by num.

## src/main/java/org/tron/core/services/http/Util.java
Add new methods:

```
public static String printBlockKafka(Block block)
public static JSONArray printTransactionListToKafkaJSON(List<TransactionCapsule> list)
public static JSONObject printTransactionToKafkaJSON(Transaction transaction) 
```

## src/main/java/org/tron/core/db/Manager.java
the main call to send data to kafka:

```
    if (!Args.getInstance().getKafkaEndpoint().equals("")) {
            try {
              logger.info("send switch fork block {} to kafka.", newBlock.getNum());
              HttpUtil.postJsonContent(Args.getInstance().getKafkaEndpoint(), Util.printBlockKafka(newBlock.getInstance()));
            } catch (Exception e) {
              logger.error(e.getMessage());
            }
    }

    if (!Args.getInstance().getKafkaEndpoint().equals("")) {
        try {
          logger.info("send block {} to kafka.", newBlock.getNum());
          HttpUtil.postJsonContent(Args.getInstance().getKafkaEndpoint(), Util.printBlockKafka(newBlock.getInstance()));
        } catch (Exception e) {
          logger.error(e.getMessage());
        }
    }

```
