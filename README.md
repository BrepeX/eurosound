# Еврозвук - личный кабинет

## Описание проекта

Проект представляет собой личный кабинет для управления аудиорекламой в общественных местах. Основные функциональности включают в себя: управление списком роликов, их архивацию, демонстрацию роликов, а также работу с лицензиями и медиапланами. Пользователи могут фильтровать ролики, переносить их между различными статусами, а также просматривать детальную информацию по каждому ролику, заказу или лицензии, переходя на соответствующую детальную страницу. Цель проекта — обеспечить удобный и эффективный инструмент для управления аудиорекламой, сокращая время и усилия, затрачиваемые на эти процессы.


## Назначение проекта

Этот проект был разработан с целью предоставления удобного и интуитивно понятного инструмента для управления аудиорекламой в общественных местах. Он решает ряд проблем и удовлетворяет потребности пользователей, связанные с необходимостью эффективного управления рекламными роликами, их архивацией, демонстрацией и лицензированием. Платформа позволяет быстро находить и редактировать нужные ролики, а также управлять их статусами, что значительно экономит время и ресурсы компании. Кроме того, детальные страницы с информацией по каждому ролику, заказу и лицензии обеспечивают прозрачность и контроль над всеми процессами внутри личного кабинета.


## Галерея

![Изображение 1](https://github.com/BrepeX/eurosound/blob/main/screen%201.png)
![Изображение 2](https://github.com/BrepeX/eurosound/blob/main/screen%202.png)
![Изображение 3](https://github.com/BrepeX/eurosound/blob/main/screen%203.png)
![Изображение 4](https://github.com/BrepeX/eurosound/blob/main/screen%204.png)
![Изображение 5](https://github.com/BrepeX/eurosound/blob/main/screen%205.png)
![Изображение 6](https://github.com/BrepeX/eurosound/blob/main/screen%206.png)

## Моя роль в проекте

В рамках данного проекта я отвечал за разработку Front-end части. Мои обязанности включали в себя реализацию пользовательского интерфейса, обеспечение его взаимодействия с серверной частью, а также интеграцию с back-end. Я тесно сотрудничал с разработчиками серверной части, чтобы гарантировать бесперебойную работу всего приложения, и оптимизировал Front-end для обеспечения высокой производительности и отзывчивости интерфейса. Мой вклад в проект также включал обеспечение корректной работы всех функций личного кабинета, отображение списков роликов, их архивации, демонстрации, а также работы с лицензиями и медиапланами. Я также уделял внимание деталям на страницах с детальной информацией по роликам, заказам и лицензиям, обеспечивая чистоту и читаемость представления данных.


## Технологии, используемые в проекте

  ![NextJS](https://img.shields.io/badge/-NextJS-black?style=for-the-badge&logo=next.js)
  ![Typescript](https://img.shields.io/badge/-Typescript-white?style=for-the-badge&logo=typescript)
  ![SCSS](https://img.shields.io/badge/-SCSS-pink?style=for-the-badge&logo=sass)
  ![Axios](https://img.shields.io/badge/-Axios-blue?style=for-the-badge)
  ![Redux Toolkit](https://img.shields.io/badge/-Redux_Toolkit-purple?style=for-the-badge&logo=redux)
  ![React Hook Form](https://img.shields.io/badge/-React_Hook_Form-blue?style=for-the-badge)
  ![Ant Design](https://img.shields.io/badge/-Ant_Design-blue?style=for-the-badge&logo=antdesign)

## Архитектура проекта

Применена архитектура Features Sliced Design (FSD), ориентированная на разделение проекта по функциональным блокам для улучшения масштабируемости и удобства поддержки.

![Изображение 7](https://github.com/BrepeX/eurosound/blob/main/screen%207.png)


## Принципы и инструменты разработки
- Код-стиль и форматирование: Prettier
- Система контроля версий: Gitlab
- Прочие инструменты: webpack config
- Линтер: ESLint
  <details>
  <summary>Конфиг linter`а</summary>
  
  ```javascript
  module.exports = {
    env: { browser: true, es2020: true, node: true },
    extends: [
      "eslint:recommended",
      "plugin:@typescript-eslint/recommended",
      "plugin:react-hooks/recommended",
      "@feature-sliced",
      "plugin:@next/next/recommended",
      "next",
    ],
    parser: "@typescript-eslint/parser",
    parserOptions: { ecmaVersion: "latest", sourceType: "module" },
    plugins: ["react-refresh"],
    rules: {
      "react-refresh/only-export-components": "off",
      "no-unused-vars": "off",
      "@typescript-eslint/no-unused-vars": [
        "error",
        { ignoreRestSiblings: true },
      ],
      "import/no-internal-modules": "off",
    },
    settings: {
      "import/resolver": {
        typescript: {
          alwaysTryTypes: true,
        },
      },
    },
  };
</details>

## Код: Предпросмотр

В данном разделе представлены отрывки кода, демонстрирующие ключевые моменты и методы, использованные в проекте. Эти фрагменты кода отражают стиль программирования, архитектурные решения и технологические практики, применённые в ходе работы.

<details>
  <summary>Компонент аудио плеера</summary>

  ```javascript
    import cs from "classnames";
    import Link from "next/link";
    import { FC, useCallback, useMemo } from "react";
    import { PlayPlayerIcon, PausePlayerIcon } from "@shared/images";
    import { useAudio } from "@shared/libs/audio";
    import { parseSecondsToInline } from "../../lib";
    import { ProgressLine } from "../progress-line/ProgressLine";
    import classes from "./PlayerControl.module.scss";
    
    interface IPlayerControlProps {
      src: string;
      id: number | string;
      title: string;
      link?: string;
      isHide?: boolean;
      className?: string;
    }
    
    export const PlayerControl: FC<IPlayerControlProps> = ({
      src,
      id,
      title,
      link,
      isHide = false,
      className,
    }) => {
      const audio = useAudio(src);
    
      const handlerProgressLine = useCallback(
        (time: number) => audio && audio.setCurrentTime(time),
        [audio],
      );
    
      const percent = useMemo(() => audio?.percent || 0, [audio]);
    
      if (!audio) return null;
    
      return (
        <div className={cs(classes.player, className)}>
          <button className={classes.actionButton} onClick={audio.actionsAudio}>
            {audio.playerState ? (
              <PlayPlayerIcon className={classes.icon} />
            ) : (
              <PausePlayerIcon className={classes.icon} />
            )}
          </button>
    
          <div className={classes.content}>
            <div className={classes.header}>
              <div className={classes.wrapperTitle}>
                {!isHide && (
                  <p className={classes.contentId}>
                    <span className={classes.selectedTitle}>ID</span>{" "}
                    <span>{id}</span>
                  </p>
                )}
    
                {link ? (
                  <Link href={link} className={cs(classes.title, classes.link)}>
                    {title}
                  </Link>
                ) : (
                  <h1 className={classes.title}>{title}</h1>
                )}
              </div>
    
              <p className={classes.passedTime}>
                {parseSecondsToInline(audio.currentTime, audio.duration)}
              </p>
            </div>
    
            <ProgressLine progress={percent} setValue={handlerProgressLine} />
          </div>
        </div>
      );
    };
  ```
  
</details>

<details>
  <summary>Компонент список аудио</summary>

  ```javascript
    import { FC } from "react";
    import { TrackItem } from "@features/clips";
    import { IResponseTracks } from "@shared/types";
    import classes from "./TracksList.module.scss";
    
    interface TracksListProps {
      tracks: IResponseTracks[];
    }
    
    export const TracksList: FC<TracksListProps> = ({ tracks }) => (
      <div className={classes.list}>
        {tracks.length === 0 && (
          <h1 className={classes.notFoundClips}>Ролики не найдены</h1>
        )}
        {tracks.map((track) => (
          <TrackItem className={classes.item} {...track} key={track.id} />
        ))}
      </div>
    );
  ```
</details>

<details>
  <summary>Компонент аудио</summary>

  ```javascript
    import cs from "classnames";
    import { FC, useCallback, useMemo } from "react";
    import {
      CustomCheckbox,
      selectedClips,
      deleteSelectedClip,
      addSelectClips,
    } from "@entities/selection-clips";
    import { PlayerControl } from "@entities/track";
    import { ArchiveIcon, DownloadIcon } from "@shared/images";
    import { useAppDispatch, useAppSelector } from "@shared/models";
    import { IResponseTracks } from "@shared/types";
    import { EnumVariants } from "@shared/types";
    import { ActionButton, InfoTag, StatusTag, VerticalLine } from "@shared/ui";
    import classes from "./TrackItem.module.scss";
    
    interface ITrackItemProps extends IResponseTracks {
      className?: string;
    }
    
    export const TrackItem: FC<ITrackItemProps> = ({
      title,
      id,
      categories,
      type,
      className,
    }) => {
      const { select, active } = useAppSelector(selectedClips);
      const dispatch = useAppDispatch();
    
      const categoriesList: string = useMemo(
        () => categories.map((category) => category.name).join(", "),
        [categories],
      );
    
      const checkTrackSelected: boolean = select.includes(id);
      const handleCheckboxClick = useCallback(() => {
        if (checkTrackSelected) {
          dispatch(deleteSelectedClip(id));
          return;
        }
    
        dispatch(addSelectClips(id));
      }, [checkTrackSelected, dispatch, id]);
    
      return (
        <div className={cs(classes.track, className)}>
          {active && (
            <CustomCheckbox
              checked={checkTrackSelected}
              className={classes.checkbox}
              onClick={handleCheckboxClick}
            />
          )}
          <div className={classes.wrapperPlayer}>
            <PlayerControl
              link={`/${id}`}
              title={title}
              id={id}
              src="https://media.geeksforgeeks.org/wp-content/uploads/20190625153922/frog.mp3"
            />
    
            <div className={classes.wrapperButtons}>
              <ActionButton className={classes.actionButton} Logo={ArchiveIcon} />
              <ActionButton className={classes.actionButton} Logo={DownloadIcon} />
            </div>
          </div>
    
          <div className={classes.tags}>
            <StatusTag className={classes.tag} variant={EnumVariants.primary}>
              В эфире
            </StatusTag>
    
            <StatusTag className={classes.tag}>В работе</StatusTag>
    
            <VerticalLine className={classes.line} />
    
            <InfoTag
              className={classes.infoTag}
              title="Тип ролика"
              description={type.name}
            />
    
            {categoriesList && (
              <>
                <VerticalLine className={classes.line} />
                <InfoTag
                  className={classes.infoTag}
                  title="Категория"
                  description={categoriesList}
                />
              </>
            )}
          </div>
        </div>
      );
    };
  ```
</details>

## Команда
- Общее количество человек: 5
- Роли в команде:
  - Разработчиков: 4
  - Дизайнеров: 1

# Основные достижения и результаты

- Успешная интеграция Front-end и Back-end частей проекта, обеспечивающая бесперебойную работу всего приложения.
- Реализация интуитивно понятного пользовательского интерфейса, который позволяет эффективно управлять аудиорекламой.
- Внедрение системы фильтрации и сортировки роликов, что значительно упрощает процесс поиска нужных данных.
- Оптимизация производительности Front-end части, гарантирующая быструю и отзывчивую работу приложения.
- Создание детальных страниц для роликов, заказов и лицензий, предоставляющих всю необходимую информацию в удобном формате.
- Повышение общей эффективности управления аудиорекламой и сокращение времени, затрачиваемого на эти процессы.


## Особые вызовы и преодоленные препятствия

- **Интеграция с Back-end**: Столкнулись с трудностями синхронизации Front-end и Back-end, решили через тесное взаимодействие команд и использование мок-данных для отладки.
- **Производительность интерфейса**: Изначальные проблемы с отзывчивостью решены оптимизацией кода и использованием ленивой загрузки.
- **Сложность фильтрации данных**: Внедрение продвинутых алгоритмов фильтрации и сортировки для ускорения поиска и улучшения пользовательского опыта.
- **Обеспечение кроссбраузерности**: Проблемы совместимости решены тщательным тестированием и корректировкой стилей и скриптов для различных браузеров.


## Ссылки

- **Демо проекта**: Доступ к демо-версии проекта строго ограничен и предоставляется исключительно клиентам компании по запросу.
- **Код проекта**: Код находится под защитой соглашения о неразглашении (НДА), из-за чего, к сожалению, не может быть предоставлен для общего доступа или просмотра.
