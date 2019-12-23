# DB_LAB1

Лабораторна робота № 1.

Ознайомлення з базовими операціями СУБД PostgreSQL

КП-72 Лемешенко Олег Олегович

Варіант 11 - Месенджер

Сутності:

1. Користувач (user)

CREATE TABLE public.user
(
    id bigint NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 9223372036854775807 CACHE 1 ),
    login character varying COLLATE pg_catalog."default" NOT NULL,
    name character varying COLLATE pg_catalog."default" NOT NULL,
    bio character varying COLLATE pg_catalog."default",
    CONSTRAINT user_pkey PRIMARY KEY (id),
    CONSTRAINT login UNIQUE (login)

)

2. Чат (chat)

CREATE TABLE public.chat
(
    id bigint NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 9223372036854775807 CACHE 1 ),
    tag character varying COLLATE pg_catalog."default" NOT NULL,
    name character varying COLLATE pg_catalog."default" NOT NULL,
    bio character varying COLLATE pg_catalog."default",
    CONSTRAINT chat_pkey PRIMARY KEY (id),
    CONSTRAINT tag UNIQUE (tag)

)

3. Відношення між користувачем та чатом (user_chat)

CREATE TABLE public.user_chat
(
    id bigint NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 9223372036854775807 CACHE 1 ),
    user_id bigint NOT NULL,
    chat_id bigint NOT NULL,
    "isAdmin" boolean DEFAULT false,
    CONSTRAINT user_chat_pkey PRIMARY KEY (id),
    CONSTRAINT chat FOREIGN KEY (chat_id)
        REFERENCES public.chat (id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID,
    CONSTRAINT "user" FOREIGN KEY (user_id)
        REFERENCES public."user" (id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID
)

4. Повідомлення (message)

CREATE TABLE public.message
(
    id bigint NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 9223372036854775807 CACHE 1 ),
    chat_id bigint NOT NULL,
    user_id bigint NOT NULL,
    text character varying COLLATE pg_catalog."default" NOT NULL,
    date timestamp(4) with time zone NOT NULL DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT message_pkey PRIMARY KEY (id),
    CONSTRAINT chat FOREIGN KEY (chat_id)
        REFERENCES public.chat (id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID,
    CONSTRAINT "user" FOREIGN KEY (user_id)
        REFERENCES public."user" (id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID
)
