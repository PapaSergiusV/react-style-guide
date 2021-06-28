## Основные правила

https://github.com/airbnb/javascript

1. Использовать более понятные переменные в однострочных функциях (map, filter, reduce, ...)
2. Использовать alias в путях импорта
3. Использовать именованые экспорты/импорты
4. Избегать any
5. Типизировать приходящие с сервера данные
6. Использовать комментарии в коде для доработок и фиксов на будущее

_(Контент дополняется)_

## Правила для компонентов (возможно стоит привести пример)

1. Делать компоненты небольшими и функциональными

   - функциональные компоненты лучше тестируются.
   - более переиспользуемы.
   - чем меньше компонент тем проще его оптимизировать.
   - проще обновлять.

2. Для нескольких классов использовать библиотеку classnames

### Пример простого компонента

index.ts

```jsx
export * from './SomeComponent';

```

types.ts

```jsx
export type ButtonType = 'primary' | 'outlined' | 'error';

export type User = {
  id: number;
  name: string;
};

```

SomeComponent.tsx

```jsx
import React, { useState } from 'react';
import cn from 'classnames';

import { AnotherComponent } from './AnotherComponent';
import { ButtonType, User } from './types';
import s from './SomeComponent.module.scss';

interface ISomeComponentProps {
  users: User[];
  onSelectUser: (userId: number) => void;
  onDelete: () => void;
  type?: ButtonType;
  className?: string;
  disabled?: boolean;
}

export const SomeComponent: React.FC<ISomeComponentProps> = (props: ISomeComponentProps): JSX.Element => {
  const { children, users, onSelectUser, className, type = 'primary', onDelete, disabled } = props;
  const [count, setCount] = useState<number>(0);

  return (
    <>
      {users.map((user: User) => (
        <AnotherComponent
          key={user.id}
          username={user.name}
          onClick={() => onSelectUser(user.id)}
        />
      ))}

      {/* Some code */}

      <button onClick={onDelete} className={cn(s.button, { [s.disabled]: disabled }, className)}>
        {children}
      </button>
    </>
  );
};
```

## Структура папки с компонентом

/SomeComponent/

- index.ts - файл для импорта. `import { SomeComponent } from '~/shared/SomeComponent'`
- types.ts - файл для хранения типов и интерфейсов, используемых только компонентом и его дочерними компонентами в этой папке
- styles.module.scss - файл стилей
- SomeComponent.tsx - сам компонент
- /AnotherComponent - компонент, используемый только компонентом SomeComponent
