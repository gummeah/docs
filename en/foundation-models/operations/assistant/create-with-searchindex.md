# Creating an assistant with a search index

{% include [assistants-preview-stage](../../../_includes/foundation-models/assistants-preview-stage.md) %}

{{ assistant-api }} is a tool for creating [AI assistants](../../concepts/assistant/index.md). It can be used to create personalized assistants, implement a generative response scenario adapted based on external information (known as _retrieval augmented generation_, or RAG), and save the model's request context.

## Getting started {#before-begin}

To use the examples:

{% list tabs group=programming_language %}

- SDK {#sdk}

  {% include [sdk-before-begin-assistants](../../../_includes/foundation-models/sdk-before-begin-assistants.md) %}

{% endlist %}

## Create an assistant {#create-assistant}

This example shows you how to create an [assistant](../../concepts/assistant/index.md) that uses information from files for responses. In the example, we will create an index for full-text search and a simplest chat.

{% list tabs group=programming_language %}

- SDK {#sdk}

  1. Download and unpack the [archive](https://storage.yandexcloud.net/doc-files/tours-example.zip) with examples of files that will be used as an additional source of information. The files contain advertising texts for tours to Bali and Kazakhstan generated by {{ gpt-pro }}.
  1. Create a file named `search-assistant.py` and paste the following code into it:

      {% include [searchindex-assistant](../../../_includes/foundation-models/examples/searchindex-assistant.md) %}

      Where:

      * `mypath`: Variable containing the path to the directory containing the files you downloaded earlier, e.g., `/Users/myuser/tours-example/`.

      {% include [sdk-code-legend](../../../_includes/foundation-models/examples/sdk-code-legend.md) %}

  1. Run the created file:

      ```bash
      python3 search-assistant.py
      ```

      The example implements the simplest chat possible: enter your requests to the assistant from your keyboard and get answers. To end the dialog, enter `exit`.

      {% cut "Estimated result" %}

      ```text
      Enter your question to the assistant:
      How much is a visa to Bali?
      Answer: 300 rubles.
      Enter your question to the assistant:
      Do I need a visa to enter Kazakhstan?
      Answer: The text above says that a passport is required to enter Kazakhstan from Russia. It does not mention a visa in the context of traveling to Kazakhstan.

      Answer: There is no information in the text about whether you need a visa to enter Kazakhstan. But you need a passport to visit this country.
      Enter your question to the assistant:
      Thank you!
      Answer: You are welcome! If you have any more questions, I will do my best to help.
      Enter your question to the assistant:
      exit
      Outputting the whole message history when exiting the chat:
          message=Message(id='fvt6lh2ng3r2********', parts=('You are welcome! If you have any more questions, I will do my best to help.',), thread_id='fvt4rk62dcic********', created_by='aje62tfcd0oj********', created_at=datetime.datetime(2024, 12, 5, 8, 0, 4, 189394), labels=None, author=Author(id='fvtdvgoissl2********', role='ASSISTANT'))
          message.text='You are welcome! If you have any more questions, I will do my best to help.'

          message=Message(id='fvt8mmj7t3bs********', parts=('Thank you!',), thread_id='fvt4rk62dcic********', created_by='aje62tfcd0oj********', created_at=datetime.datetime(2024, 12, 5, 8, 0, 2, 500379), labels=None, author=Author(id='fvtcqfte9ih3********', role='USER'))
          message.text='Thank you!'

          message=Message(id='fvte2e8sl7be********', parts=('The text above says that a passport is required to enter Kazakhstan from Russia. It does not mention a visa in the context of traveling to Kazakhstan.\n\nAnswer: There is no information in the text about whether you need a visa to enter Kazakhstan. But you need a passport to visit this country.',), thread_id='fvt4rk62dcic********', created_by='aje62tfcd0oj********', created_at=datetime.datetime(2024, 12, 5, 7, 59, 38, 449794), labels=None, author=Author(id='fvtdvgoissl2********', role='ASSISTANT'))
          message.text='The text above says that a passport is required to enter Kazakhstan from Russia. It does not mention a visa in the context of traveling to Kazakhstan.\n\nAnswer: There is no information in the text about whether you need a visa to enter Kazakhstan. But you need a passport to visit this country.'

          message=Message(id='fvtm404ffbo9********', parts=('Do I need a visa to enter Kazakhstan?',), thread_id='fvt4rk62dcicf81e5g5c', created_by='aje62tfcd0oj********', created_at=datetime.datetime(2024, 12, 5, 7, 59, 35, 300522), labels=None, author=Author(id='fvtcqfte9ih3********', role='USER'))
          message.text='Do I need a visa to enter Kazakhstan?'

          message=Message(id='fvtljfirc36m********', parts=('300 rubles.',), thread_id='fvt4rk62dcic********', created_by='aje62tfcd0oj********', created_at=datetime.datetime(2024, 12, 5, 7, 59, 17, 736961), labels=None, author=Author(id='fvtdvgoissl2********', role='ASSISTANT'))
          message.text='300 rubles.'

          message=Message(id='fvt3rc4oat2m********', parts=('How much is a visa to Bali?',), thread_id='fvt4rk62dcic********', created_by='aje62tfcd0oj********', created_at=datetime.datetime(2024, 12, 5, 7, 59, 16, 599110), labels=None, author=Author(id='fvtcqfte9ih3********', role='USER'))
          message.text='How much is a visa to Bali?'
      ```

      {% endcut %}

{% endlist %}

#### See also {#see-also}

* [{#T}](./create.md)
* Examples of working with ML SDK on [GitHub](https://github.com/yandex-cloud/yandex-cloud-ml-sdk/tree/master/examples/sync/assistants)