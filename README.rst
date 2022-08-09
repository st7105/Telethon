Forked Telethon |logo|
======================
.. |logo| image:: https://github.com/LonamiWebs/Telethon/raw/master/logo.svg
    :width: 60pt
    :height: 60pt

`↗️ Updated tl.telethon.dev <https://disk6969.github.io/Telethon>`_

.. code-block:: py

  +-----------------------+
  |   Telethon 1.24.0     |
  +-----------------------+
  |      layer: 144       |
  +-----------------------+

About
=====

A simple clone of the awesome Telegram MTproto client version 1.24.0 but with up to date components, 
You do not have to change previous code with telethon, as there are no breaking changes.

install: (if any issues, try uninstalling telethon first):

.. code-block:: py

  pip install -U newthon


Reactions
=========

.. code-block:: py

    client.send_reaction(chat, message, "😢")

or shorter:

.. code-block:: py

    message.react("😁", big=True)

to send a reaction with animation(for pms) use `big=True`, and, to remove a reaction use `remove=True`: 

.. code-block:: py

    message.react(remove=True)

Premium
=======
- You can send files larger that 2GiB through Telegram
- Premium users will have .premium in their user object
- Premium stickers will also have .premium that might need dealing if you don't have subscription.

Requests of join and events for ChatAction events
=================================================
* event.new_invite (only for bot accounts)

.. code-block:: py

    @bot.on(events.ChatAction(func=lambda e : e.new_join_request))
    async def _(event):
        event.approve_user(approved=True or False)


* event.new_approve for user accounts

.. code-block:: py

    @client.on(events.ChatAction(func=lambda e : e.new_approve))
    async def _(event):
        event.approve_user(approved=True/False)


using raw api to accept old requests
------------------------------------

- Getting them

.. code-block:: py

    result = client(functions.messages.GetChatInviteImportersRequest(
        peer="chat",
        offset_date=None, 
        offset_user=telethon.tl.types.InputUserEmpty(),
        limit=1000
    ))

- manual approve

.. code-block:: py

    for a in result:
        client(functions.messages.HideChatJoinRequestRequest(
            peer='chat or username',
            user_id='To-approve',
            approved=True or False
        ))


- batch approve: 

.. code-block:: py 

    client(functions.messages.HideAllChatJoinRequestsRequest(
        peer=entity, 
        approved=True or False
    ))

iter_participant
================
aggressive True will sleep by default.
its sleep value can be adjusted using the sleep parameter, this will make it sleep for that specified amount before processing next chunk.

.. code-block:: py 

    client.get_participant(chat, aggressive=True, sleep=2)

WebView Button
===============
You can input a web bot button as an inline button or a keyboard button, sine it can be both.
the default is inline button, you can use the inline=False to use it in a keyboard button

.. code-block:: py

    from telethon import Button
    client.send_message(chat, "Open Google", buttons=Button.web("google", "https://google.com")

- note that webapp keyboard can be only a single button, it won't allow others with it.

.. code-block:: py

    client.send_message(chat, "YouTube", buttons=Button.web("google", "https://YouTube.com", inline=False)

Content privacy
===============
``chat.noforwards`` will return True for chats with forward restriction enabled, same applies to bot messages with ``message.noforwards``
You can use the argument ``noforwards=True`` in sender methods.

.. code-block:: py

    client.send_message(chat, "lonami is god", noforwards=True)
    
spoilers
========
You can use `||Text||` to create spoilers, or, for HTML `<tg-spoiler>Text</tg-spoiler>`

to create underline markdown, use --Text--


links in get message
====================
also you can now get a single message using the link in get/iter_messages()

``client.get_messages("https://t.me/username/1")``

the message object will also have .link, which will return link of the message
