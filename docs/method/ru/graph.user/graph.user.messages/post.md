graph.user.messages

$description
Создание новых сообщений и действия над чатами и диалогами

$payload


$additional
С помощью данного метода можно совершить ряд действий над чатом / диалог, а именно:

* создавать новые сообщения;
* управлять состоянием чата / диалога;
* управлять участниками чата / диалога;
* оповещать других пользователей о состоянии текущего пользователя.

**Пример запроса**

{% format_code https://api.ok.ru/graph/me/messages/chat:C3ecb9d02a600?access_token=tkn18YdUJZe:CQABPOJKAKEKEKEKE %}

**Управление состоянием чата**

Можно управлять следующими параметрами чата:

* название чата;
* иконка / логотип чата. 

Пример запроса для изменения названия чата:
{% highlight json %}
{
  "recipient":{"chat_id":"chat:C3ecb9d02a600"},           /* ID чата в формате chat:id */
  "chat_control":{
    "title":"Наш уютный чатик"                            /* Новый заголовок */
  }
}
{% endhighlight %}

Пример запроса для изменения иконки чата:
{% highlight json %}
{
  "recipient":{"chat_id":"chat:C3ecb9d02a600"},                 /* ID чата в формате chat:id */
  "chat_control":{
    "icon":{"url":"https://and.su/tamtam/icon128.png"}          /* URL иконки в формате jpg или png */
  }
}
{% endhighlight %}

**Управление участниками**

Для участников чата доступны следующие действие:

* добавление участника в чат;
* удаление участника из чата;
* выход из чата

Пример добавления участника в чат:

{% highlight json %}
{
  "recipient":{"chat_id":"chat:C3ecb9d02a600"},         /* ID чата в формате chat:id*/
  "chat_control":{
    "add_members":[                                     /* Массив пользователей для добавления */
      {"user_id":"user:123456789012"},                  /* ID пользователя для добавления в чат в формате user:id */
      {"user_id":"user:123456789021"}
    ]
  }
}
{% endhighlight %}

Пример удаления участника из чата:

{% highlight json %}
{
  "recipient":{"chat_id":"chat:C3ecb9d02a600"},             /* ID чата в формате chat:id */
  "chat_control":{
    "remove_member":{"user_id":"user:123456789012"}         /* ID пользователя для удаления из чата в формате user:id */
  }
}
{% endhighlight %}

Пример запроса для выхода участником из чата:

{% highlight json %}
{
  "recipient":{"chat_id":"chat:C3ecb9d02a600"},             /* ID чата в формате chat:id */
  "chat_control":{
    "leave":"true"                                          /* Действие выхода из чата */
  }
}
{% endhighlight %}

**Создание новых сообщений**

Создание новых сообщений - ключевое действие для данного метода.

Чтобы создать новое сообщение, надо передать в POST-payload метода объект сообщения, который соотвествует по структуре 
объекту сообщения из метода /me/messages/get.

Пример текстового сообщения:

{% highlight json %}
{
  "recipient":{"user_id":"chat:C3ecb9d02a600"},          /* ID чата в формате chat:id */
  "message":{                                           /* Содержание сообщения */
    "text":"Привет"                                     /* Текст сообщения */
  }
}
{% endhighlight %}

Также в данный момент можно отослать сообщение с приложением типа IMAGE:
{% highlight json %}
{
  "recipient":{"chat_id":"chat:C3ecb9d02a600"},                     /* ID чата в формате chat:id */
  "message":{                                                       /* Содержание сообщения */
    "attachment":{
      "type":"IMAGE",
      "payload":{"url":"https://st.mycdn.me/res/i/ok_logo.png"}     /* URL изображения в формате jpg или png */
    }
  }
}
{% endhighlight %}

Поддержка других типов приложений (VIDEO, AUDIO, SHARE) будет добавлена в будущем.

**Состояние текущего пользователя**

Текущий пользователь может оповестить других участников о следующих действиях:

* отметить все сообщения как прочитанные - mark_seen;
* пользователь печатает новое сообщение - typing_on;
* пользователь отсылает новый файл (видео, аудио, фото) - sending_photo.

Пример запроса: 

{% highlight json %}
{
  "recipient":{"chat_id":"chat:C3ecb9d02a600"},     /* ID чата или диалога */
  "sender_action":"mark_seen"                       /* Возможные значения: sending_photo, sending_video, sending_audio */
}
{% endhighlight %}

**Отсылка сообщения пользователям**

С помощью данного метода можно произвести отсылку сообщения от группы одному или нескольким пользователям сразу.

Для успешной отсылки сообщения пользователь должен разрешить группе отсылать ему сообщения.

Чтобы отослать такое сообщение, необходимо при вызове метода передать сообщение со следующим содержимым

{% highlight json %}
{
    "recipient":{
        "user_ids":[                                /* Список id пользователей в формате user:id или id*/
            "user:123456789012",                    
            "210987654321"
        ]
    },
    "message":{                                     /* Содержимое сообщения */
        "text":"Hello"                              /* Текст сообщения */
    }
}
{% endhighlight %}

Каждый пользователь получит это сообщение в своём персональном чате между пользователем и группой.

API в качестве ответа отдаст список boolean значений, говорящих о том, было ли успешно отослано сообщение соответствующему пользователю.
Порядок этих значений соответствует порядку указания пользователей в запросе. 
{% highlight json %}
{
  "success": [                                          
    false,
    false
  ]
}
{% endhighlight %}