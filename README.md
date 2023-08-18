# bedrock-stream

## Bedrock API를 활용

[invoke_model_with_response_stream](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_InvokeModelWithResponseStream.html)을 이용하여 아래와 같이 stream으로 결과를 얻을 수 있습니다.

```python
bedrock_region = "us-west-2" 
bedrock_config = {
  "region_name":bedrock_region,
  "endpoint_url":"https://prod.us-west-2.frontend.bedrock.aws.dev"
}

bedrock_client = boto3.client(
service_name='bedrock',
region_name=bedrock_config["region_name"],
endpoint_url=bedrock_config["endpoint_url"]

response = bedrock_client.invoke_model_with_response_stream(body=body, modelId=modelId, accept=accept, contentType=contentType)
stream = response.get('body')
output = []
i = 1
if stream:
    for event in stream:
        chunk = event.get('chunk')
        if chunk:
            chunk_obj = json.loads(chunk.get('bytes').decode())
            text = chunk_obj['outputText']
            output.append(text)
            print(f'\t\t\x1b[31m**Chunk {i}**\x1b[0m\n{text}\n')
            i+=1
```

이때의 결과는 아래와 같습니다. 

```text
		**Chunk 1**
 "Jane Doe"
Subject: Apology for Poor Service Experience

Dear John Doe,

I hope this email finds you well. I am writing to express my sincere apologies for the poor service experience you had when

		**Chunk 2**
 you contacted our customer support engineer Jane Doe.

We strive to provide excellent service to all our customers, and it is disheartening to hear that we fell short of your expectations. We take feedback like yours very seriously, and we are committed to resolving the issue and ensuring that we provide better service in the future.

If

		**Chunk 3**
 you would like to discuss this matter further, please do not hesitate to contact me directly. We value your opinion and want to make things right for you.

Once again, I apologize for any inconvenience caused.

Best regards,

Bob Customer Service Manager
```

## LangChain으로 Stream 처리

[관련한 github](https://github.com/langchain-ai/langchain/issues/9094)을 보면 현재 Open 이슈로 개발 단계로 보여집니다.

[OpenAI Stream](https://python.langchain.com/docs/modules/model_io/models/chat/streaming)과 같은 형태로 제공되어야 할것 같습니다.

