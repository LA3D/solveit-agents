cosette

  1. Tool loop

  * cosette

  * Cosette’s source

  * Tool loop

## On this page

  * Chat.toolloop

  * __Report an issue

## Other Formats

  *  __CommonMark

# Tool loop

  * Show All Code
  * Hide All Code

```
model = models[0]
```

```
orders = {
    "O1": dict(id="O1", product="Widget A", quantity=2, price=19.99, status="Shipped"),
    "O2": dict(id="O2", product="Gadget B", quantity=1, price=49.99, status="Processing"),
    "O3": dict(id="O3", product="Gadget B", quantity=2, price=49.99, status="Shipped")}

customers = {
    "C1": dict(name="John Doe", email="john@example.com", phone="123-456-7890",
               orders=[orders['O1'], orders['O2']]),
    "C2": dict(name="Jane Smith", email="jane@example.com", phone="987-654-3210",
               orders=[orders['O3']])
}
```

```
def get_customer_info(
    customer_id:str # ID of the customer
): # Customer's name, email, phone number, and list of orders
    "Retrieves a customer's information and their orders based on the customer ID"
    print(f'- Retrieving customer {customer_id}')
    return customers.get(customer_id, "Customer not found")

def get_order_details(
    order_id:str # ID of the order
): # Order's ID, product name, quantity, price, and order status
    "Retrieves the details of a specific order based on the order ID"
    print(f'- Retrieving order {order_id}')
    return orders.get(order_id, "Order not found")

def cancel_order(
    order_id:str # ID of the order to cancel
)->bool: # True if the cancellation is successful
    "Cancels an order based on the provided order ID"
    print(f'- Cancelling order {order_id}')
    if order_id not in orders: return False
    orders[order_id]['status'] = 'Cancelled'
    return True
```

```
tools = [get_customer_info, get_order_details, cancel_order]
chat = Chat(model, tools=tools)
```

```
r = chat('Can you tell me the email address for customer C2?')
```

```
- Retrieving customer C2
```

```
choice = r.choices[0]
print(choice.finish_reason)
choice
```

```
tool_calls
```

```
Choice(finish_reason='tool_calls', index=0, logprobs=None, message=ChatCompletionMessage(content=None, role='assistant', function_call=None, tool_calls=[ChatCompletionMessageToolCall(id='call_BYev4ExQk899v9yjLERyDGSc', function=Function(arguments='{"customer_id":"C2"}', name='get_customer_info'), type='function')]))
```

```
r = chat()
r
```

The email address for customer C2 (Jane Smith) is jane@example.com.

  * id: chatcmpl-9R81d5i8pBSIRg5B7erJcF8t5BzKw
  * choices: [Choice(finish_reason=‘stop’, index=0, logprobs=None, message=ChatCompletionMessage(content=‘The email address for customer C2 (Jane Smith) is jane@example.com.’, role=‘assistant’, function_call=None, tool_calls=None))]
  * created: 1716252733
  * model: gpt-4o-2024-05-13
  * object: chat.completion
  * system_fingerprint: fp_729ea513f7
  * usage: CompletionUsage(completion_tokens=17, prompt_tokens=251, total_tokens=268)

```
chat = Chat(model, tools=tools)
r = chat('Please cancel all orders for customer C1 for me.')
print(r.choices[0].finish_reason)
find_block(r)
```

```
- Retrieving customer C1
tool_calls
```

```
ChatCompletionMessage(content=None, role='assistant', function_call=None, tool_calls=[ChatCompletionMessageToolCall(id='call_lENp0aVeTJ7W0q8SRgC7h5s9', function=Function(arguments='{"customer_id":"C1"}', name='get_customer_info'), type='function')])
```

* * *

source

### Chat.toolloop

>
```
>      Chat.toolloop (pr, max_steps=10, trace_func:Optional[<built-
>                     infunctioncallable>]=None, cont_func:Optional[<built-
>                     infunctioncallable>]=<function noop>, audio:Optional[ChatC
>                     ompletionAudioParam]|NotGiven=NOT_GIVEN,
>                     frequency_penalty:Optional[float]|NotGiven=NOT_GIVEN, func
>                     tion_call:completion_create_params.FunctionCall|NotGiven=N
>                     OT_GIVEN, functions:Iterable[completion_create_params.Func
>                     tion]|NotGiven=NOT_GIVEN,
>                     logit_bias:Optional[Dict[str,int]]|NotGiven=NOT_GIVEN,
>                     logprobs:Optional[bool]|NotGiven=NOT_GIVEN,
>                     max_completion_tokens:Optional[int]|NotGiven=NOT_GIVEN,
>                     max_tokens:Optional[int]|NotGiven=NOT_GIVEN,
>                     metadata:Optional[Dict[str,str]]|NotGiven=NOT_GIVEN, modal
>                     ities:Optional[List[ChatCompletionModality]]|NotGiven=NOT_
>                     GIVEN, n:Optional[int]|NotGiven=NOT_GIVEN,
>                     parallel_tool_calls:bool|NotGiven=NOT_GIVEN, prediction:Op
>                     tional[ChatCompletionPredictionContentParam]|NotGiven=NOT_
>                     GIVEN,
>                     presence_penalty:Optional[float]|NotGiven=NOT_GIVEN, reaso
>                     ning_effort:ChatCompletionReasoningEffort|NotGiven=NOT_GIV
>                     EN, response_format:completion_create_params.ResponseForma
>                     t|NotGiven=NOT_GIVEN,
>                     seed:Optional[int]|NotGiven=NOT_GIVEN, service_tier:"Optio
>                     nal[Literal['auto','default']]|NotGiven"=NOT_GIVEN,
>                     stop:Union[Optional[str],List[str]]|NotGiven=NOT_GIVEN,
>                     store:Optional[bool]|NotGiven=NOT_GIVEN, stream:Optional[L
>                     iteral[False]]|Literal[True]|NotGiven=NOT_GIVEN, stream_op
>                     tions:Optional[ChatCompletionStreamOptionsParam]|NotGiven=
>                     NOT_GIVEN, temperature:Optional[float]|NotGiven=NOT_GIVEN,
>                     tool_choice:ChatCompletionToolChoiceOptionParam|NotGiven=N
>                     OT_GIVEN, tools:Iterable[ChatCompletionToolParam]|NotGiven
>                     =NOT_GIVEN, top_logprobs:Optional[int]|NotGiven=NOT_GIVEN,
>                     top_p:Optional[float]|NotGiven=NOT_GIVEN,
>                     user:str|NotGiven=NOT_GIVEN,
>                     extra_headers:Headers|None=None,
>                     extra_query:Query|None=None, extra_body:Body|None=None,
>                     timeout:float|httpx.Timeout|None|NotGiven=NOT_GIVEN)
```

_Add prompt`pr` to dialog and get a response from the model, automatically following up with `tool_use` messages_

| **Type** | **Default** | **Details**  
---|---|---|---  
pr |  |  | Prompt to pass to model  
max_steps | int | 10 | Maximum number of tool requests to loop through  
trace_func | Optional | None | Function to trace tool use steps (e.g `print`)  
cont_func | Optional | noop | Function that stops loop if returns False  
audio | Optional[ChatCompletionAudioParam] | NotGiven | NOT_GIVEN |   
frequency_penalty | Optional[float] | NotGiven | NOT_GIVEN |   
function_call | completion_create_params.FunctionCall | NotGiven | NOT_GIVEN |   
functions | Iterable[completion_create_params.Function] | NotGiven | NOT_GIVEN |   
logit_bias | Optional[Dict[str, int]] | NotGiven | NOT_GIVEN |   
logprobs | Optional[bool] | NotGiven | NOT_GIVEN |   
max_completion_tokens | Optional[int] | NotGiven | NOT_GIVEN |   
max_tokens | Optional[int] | NotGiven | NOT_GIVEN |   
metadata | Optional[Dict[str, str]] | NotGiven | NOT_GIVEN |   
modalities | Optional[List[ChatCompletionModality]] | NotGiven | NOT_GIVEN |   
n | Optional[int] | NotGiven | NOT_GIVEN |   
parallel_tool_calls | bool | NotGiven | NOT_GIVEN |   
prediction | Optional[ChatCompletionPredictionContentParam] | NotGiven | NOT_GIVEN |   
presence_penalty | Optional[float] | NotGiven | NOT_GIVEN |   
reasoning_effort | ChatCompletionReasoningEffort | NotGiven | NOT_GIVEN |   
response_format | completion_create_params.ResponseFormat | NotGiven | NOT_GIVEN |   
seed | Optional[int] | NotGiven | NOT_GIVEN |   
service_tier | Optional[Literal[‘auto’, ‘default’]] | NotGiven | NOT_GIVEN |   
stop | Union[Optional[str], List[str]] | NotGiven | NOT_GIVEN |   
store | Optional[bool] | NotGiven | NOT_GIVEN |   
stream | Optional[Literal[False]] | Literal[True] | NotGiven | NOT_GIVEN |   
stream_options | Optional[ChatCompletionStreamOptionsParam] | NotGiven | NOT_GIVEN |   
temperature | Optional[float] | NotGiven | NOT_GIVEN |   
tool_choice | ChatCompletionToolChoiceOptionParam | NotGiven | NOT_GIVEN |   
tools | Iterable[ChatCompletionToolParam] | NotGiven | NOT_GIVEN |   
top_logprobs | Optional[int] | NotGiven | NOT_GIVEN |   
top_p | Optional[float] | NotGiven | NOT_GIVEN |   
user | str | NotGiven | NOT_GIVEN |   
extra_headers | Headers | None | None |   
extra_query | Query | None | None |   
extra_body | Body | None | None |   
timeout | float | httpx.Timeout | None | NotGiven | NOT_GIVEN |   
  
Exported source

```
@patch
@delegates(Completions.create)
def toolloop(self:Chat,
             pr, # Prompt to pass to model
             max_steps=10, # Maximum number of tool requests to loop through
             trace_func:Optional[callable]=None, # Function to trace tool use steps (e.g `print`)
             cont_func:Optional[callable]=noop, # Function that stops loop if returns False
             **kwargs):
    "Add prompt `pr` to dialog and get a response from the model, automatically following up with `tool_use` messages"
    r = self(pr, **kwargs)
    for i in range(max_steps):
        ch = r.choices[0]
        if ch.finish_reason!='tool_calls': break
        if trace_func: trace_func(r)
        r = self(**kwargs)
        if not (cont_func or noop)(self.h[-2]): break
    if trace_func: trace_func(r)
    return r
```

```
chat = Chat(model, tools=tools)
r = chat.toolloop('Please cancel all orders for customer C1 for me.', trace_func=print)
r
```

```
- Retrieving customer C1
ChatCompletion(id='chatcmpl-9R81fvatrbbrAuUjZRTPKWgntsRCx', choices=[Choice(finish_reason='tool_calls', index=0, logprobs=None, message=ChatCompletionMessage(content=None, role='assistant', function_call=None, tool_calls=[ChatCompletionMessageToolCall(id='call_Yss1rLc1tU2wDkWPMGSGgdor', function=Function(arguments='{"customer_id":"C1"}', name='get_customer_info'), type='function')]))], created=1716252735, model='gpt-4o-2024-05-13', object='chat.completion', system_fingerprint='fp_729ea513f7', usage=In: 157; Out: 17; Total: 174)
- Cancelling order O1
- Cancelling order O2
ChatCompletion(id='chatcmpl-9R81gjtdy8L3cUI4o46MDeeOdaaRb', choices=[Choice(finish_reason='tool_calls', index=0, logprobs=None, message=ChatCompletionMessage(content=None, role='assistant', function_call=None, tool_calls=[ChatCompletionMessageToolCall(id='call_4AIzkbEYXTGNqd6uMsPLuVEB', function=Function(arguments='{"order_id": "O1"}', name='cancel_order'), type='function'), ChatCompletionMessageToolCall(id='call_Px4E3w1qEJZCIrU5ymf6qsNt', function=Function(arguments='{"order_id": "O2"}', name='cancel_order'), type='function')]))], created=1716252736, model='gpt-4o-2024-05-13', object='chat.completion', system_fingerprint='fp_729ea513f7', usage=In: 283; Out: 48; Total: 331)
ChatCompletion(id='chatcmpl-9R81hQpOI0m1ExNidpdMrIiMx78pd', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='Both orders for customer C1 have been successfully canceled.', role='assistant', function_call=None, tool_calls=None))], created=1716252737, model='gpt-4o-2024-05-13', object='chat.completion', system_fingerprint='fp_729ea513f7', usage=In: 347; Out: 12; Total: 359)
```

Both orders for customer C1 have been successfully canceled.

  * id: chatcmpl-9R81hQpOI0m1ExNidpdMrIiMx78pd
  * choices: [Choice(finish_reason=‘stop’, index=0, logprobs=None, message=ChatCompletionMessage(content=‘Both orders for customer C1 have been successfully canceled.’, role=‘assistant’, function_call=None, tool_calls=None))]
  * created: 1716252737
  * model: gpt-4o-2024-05-13
  * object: chat.completion
  * system_fingerprint: fp_729ea513f7
  * usage: CompletionUsage(completion_tokens=12, prompt_tokens=347, total_tokens=359)

```
chat.toolloop('What is the status of order O2?')
```

```
- Retrieving order O2
```

The status of order O2 is “Cancelled”.

  * id: chatcmpl-9R81inXsiw5xzoux1xw5Z7bBoPsuN
  * choices: [Choice(finish_reason=‘stop’, index=0, logprobs=None, message=ChatCompletionMessage(content=‘The status of order O2 is “Cancelled”.’, role=‘assistant’, function_call=None, tool_calls=None))]
  * created: 1716252738
  * model: gpt-4o-2024-05-13
  * object: chat.completion
  * system_fingerprint: fp_729ea513f7
  * usage: CompletionUsage(completion_tokens=11, prompt_tokens=436, total_tokens=447)

  * __Report an issue
