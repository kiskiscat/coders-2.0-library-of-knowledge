# Что такое и с чем едят

Тесты - это не только инструмент для поиска багов, но и важная часть процесса создания качественного кода. Вот почему они так сильно влияют на архитектуру.

Тестирование часто воспринимается просто как способ проверки работоспособности кода. Однако его роль гораздо глубже - тесты являются важнейшим инструментом формирования качественной архитектуры.

## Зачем нужны тесты?

### Гарантия качества

В современной разработке гарантия качества играет критическую роль. Тесты служат надёжным механизмом проверки того, что код не только работает сейчас, но и продолжит работать правильно после внесения изменений. Когда команда вносит изменения в код, автоматические тесты немедленно сигнализируют о любых нарушениях в работе системы, значительно снижая вероятность того, что проблемы достигнут производственной среды. Это особенно важно в современных условиях непрерывной поставки (Continuous Delivery), где код может попасть в продакшен несколько раз в день.

### Обратная связь

Система тестирования предоставляет разработчикам механизм мгновенной обратной связи. Это позволяет обнаруживать и исправлять ошибки на самых ранних этапах разработки, когда их исправление стоит минимальных усилий и ресурсов.

Автоматизированное тестирование также существенно сокращает время, которое обычно тратится на ручное тестирование. Вместо того чтобы раз за разом проверять базовую функциональность вручную, разработчики могут сосредоточиться на более сложных сценариях использования и креативных аспектах разработки.

### Уверенность при рефакторинге

Наличие надёжного набора тестов даёт разработчикам свободу действий при улучшении кодовой базы. Вы можете смело двигаться вперёд, зная, что всегда найдёте правильный путь. Когда код покрыт тестами, разработчики могут безбоязненно проводить масштабные изменения, будучи уверенными, что тесты немедленно укажут на любые возникшие проблемы. Это особенно важно в больших проектах, где изменение одной части системы может иметь неожиданные последствия в других её частях.

### Часть архитектуры

Тестируемость кода тесно связана с качеством архитектуры. Код, который легко тестировать, обычно обладает всеми признаками хорошего архитектурного решения: чёткими границами между модулями, минимальной сцепленностью и высокой связанностью внутри каждого модуля. Это похоже на хорошо спроектированный город, где каждый район имеет свое назначение, а транспортная система обеспечивает эффективное взаимодействие между ними. Когда код трудно тестировать, это обычно является признаком архитектурных проблем, требующих внимания.

### Документация

Тесты выполняют роль живой, всегда актуальной документации системы. В отличие от традиционной документации, которая может устареть уже на следующий день после написания, тесты всегда отражают реальное поведение системы. Они показывают, как различные компоненты должны взаимодействовать, какие входные данные ожидаются и какие результаты должны быть получены. Это особенно ценно для новых членов команды, которые могут изучить поведение системы, просто просматривая тесты. Они также служат контрактом между различными частями системы, чётко определяя ожидаемое поведение каждого компонента.

Все эти аспекты тестирования взаимосвязаны и работают сообща, создавая надёжную основу. Тесты не только помогают обнаруживать ошибки, но и направляют разработчиков к созданию более качественного, поддерживаемого и надёжного кода.

## Как писать тестируемый код?

Давайте рассмотрим принципы тестируемого кода, используя примеры на TypeScript.

### Принципы тестируемого кода

Есть множество, но рассмотрим самые применимые: SRP, DI и использование чистых функций (где это возможно).

**Single Responsibility Principle (SRP)**: один класс/функция выполняет одну задачу.

Например:

```tsx
// Типы данных для работы с пользователями
interface User {
  id: string;
  email: string;
  password: string;
  profile: UserProfile;
}

interface UserProfile {
  name: string;
  bio: string;
}

interface AuthResult {
  success: boolean;
  token?: string;
  error?: string;
}

interface AuthenticationService {
  verify(email: string, password: string): Promise<AuthResult>;
}

interface UserStorage {
  updateProfile(userId: string, profile: UserProfile): Promise<void>;
}

interface NotificationService {
  send(userId: string, message: string): Promise<void>;
}

// Используем отдельные функции с четкой ответственностью

// Функция для аутентификации
async function authenticateUser(
  email: string,
  password: string,
  authService: AuthenticationService
): Promise<AuthResult> {
  // Чистая функция аутентификации, использующая внедренный сервис
  return authService.verify(email, password);
}

// Функция для обновления профиля
async function updateUserProfile(
  userId: string,
  profile: UserProfile,
  storage: UserStorage
): Promise<void> {
  // Функция обновления профиля, использующая абстракцию хранилища
  await storage.updateProfile(userId, profile);
}

// Функция для отправки уведомлений
async function sendUserNotification(
  userId: string,
  message: string,
  notifier: NotificationService
): Promise<void> {
  // Функция отправки уведомлений, использующая сервис уведомлений
  await notifier.send(userId, message);
}
```

Теперь напишем тесты для них используя Vitest:

```tsx
// Импортируем необходимые функции из Vitest
import { describe, it, expect, vi } from 'vitest';

// Тесты для аутентификации пользователя
describe('authenticateUser', () => {
  it('should successfully authenticate a user with valid credentials', async () => {
    const mockAuthService: AuthenticationService = {
      verify: vi.fn().mockResolvedValue({
        success: true,
        token: 'valid-token-123',
      }),
    };

    const result = await authenticateUser(
      'test@example.com',
      'correct-password',
      mockAuthService
    );

    // Проверки в Vitest синтаксически идентичны Jest
    expect(result.success).toBe(true);
    expect(result.token).toBe('valid-token-123');

    // Проверяем корректность вызова мока
    expect(mockAuthService.verify).toHaveBeenCalledWith(
      'test@example.com',
      'correct-password'
    );
    // Дополнительная проверка количества вызовов
    expect(mockAuthService.verify).toHaveBeenCalledOnce();
  });

  it('should fail authentication with invalid credentials', async () => {
    const mockAuthService: AuthenticationService = {
      verify: vi.fn().mockResolvedValue({
        success: false,
        error: 'Invalid credentials',
      }),
    };

    const result = await authenticateUser(
      'test@example.com',
      'wrong-password',
      mockAuthService
    );

    expect(result.success).toBe(false);
    expect(result.error).toBe('Invalid credentials');
  });

  it('should handle authentication service errors', async () => {
    const mockAuthService: AuthenticationService = {
      verify: vi.fn().mockRejectedValue(new Error('Service unavailable')),
    };

    // В Vitest мы можем использовать более современный синтаксис для проверки ошибок
    await expect(async () => {
      await authenticateUser('test@example.com', 'password', mockAuthService);
    }).rejects.toThrowError('Service unavailable');
  });
});

// Тесты для обновления профиля пользователя
describe('updateUserProfile', () => {
  it('should successfully update user profile', async () => {
    const mockStorage: UserStorage = {
      updateProfile: vi.fn().mockResolvedValue(undefined),
    };

    const testProfile: UserProfile = {
      name: 'John Doe',
      bio: 'Test bio',
    };

    await updateUserProfile('user-123', testProfile, mockStorage);

    // Vitest предоставляет улучшенный вывод ошибок при проверке вызовов
    expect(mockStorage.updateProfile).toHaveBeenCalledWith(
      'user-123',
      testProfile
    );
    expect(mockStorage.updateProfile).toHaveBeenCalledOnce();
  });

  it('should handle storage errors during profile update', async () => {
    const mockStorage: UserStorage = {
      updateProfile: vi
        .fn()
        .mockRejectedValue(new Error('Storage update failed')),
    };

    const testProfile: UserProfile = {
      name: 'John Doe',
      bio: 'Test bio',
    };

    await expect(async () => {
      await updateUserProfile('user-123', testProfile, mockStorage);
    }).rejects.toThrowError('Storage update failed');
  });
});

// Тесты для отправки уведомлений
describe('sendUserNotification', () => {
  it('should successfully send notification', async () => {
    const mockNotifier: NotificationService = {
      send: vi.fn().mockResolvedValue(undefined),
    };

    const testMessage = 'Test notification message';

    await sendUserNotification('user-123', testMessage, mockNotifier);

    expect(mockNotifier.send).toHaveBeenCalledWith('user-123', testMessage);
    expect(mockNotifier.send).toHaveBeenCalledOnce();
  });

  it('should handle notification service errors', async () => {
    const mockNotifier: NotificationService = {
      send: vi
        .fn()
        .mockRejectedValue(new Error('Notification service unavailable')),
    };

    await expect(async () => {
      await sendUserNotification('user-123', 'test message', mockNotifier);
    }).rejects.toThrowError('Notification service unavailable');
  });
});
```

**Dependency Injection (DI)**: зависимости передаются извне, а не создаются внутри.

Например:

```tsx
// Определяем типы для работы с заказами
interface Order {
  id: string;
  items: OrderItem[];
  total: number;
}

interface OrderItem {
  productId: string;
  quantity: number;
  price: number;
}

interface OrderStorage {
  save(order: Order): Promise<void>;
}

// Функция обработки заказа
async function processOrder(
  order: Order,
  storage: OrderStorage
): Promise<void> {
  // Функция принимает зависимость как параметр
  await storage.save(order);
}

// Функция создания заказа с вычислением итоговой суммы
function createOrder(items: OrderItem[], generateId: () => string): Order {
  // Обратите внимание, как мы внедряем даже генерацию ID
  return {
    id: generateId(),
    items,
    total: calculateOrderTotal(items),
  };
}

// Чистая функция для расчета суммы заказа
function calculateOrderTotal(items: OrderItem[]): number {
  return items.reduce((sum, item) => sum + item.price * item.quantity, 0);
}
```

Теперь напишем тесты для них:

```tsx
import { describe, it, expect, vi } from 'vitest';

// Начнем с тестирования самой простой, чистой функции calculateOrderTotal
describe('calculateOrderTotal', () => {
  it('should correctly calculate total for a single item', () => {
    // Создаем простой тестовый случай с одним товаром
    const items: OrderItem[] = [
      { productId: 'prod-1', quantity: 2, price: 10 },
    ];

    const total = calculateOrderTotal(items);

    // Проверяем, что расчет выполнен правильно: 2 * 10 = 20
    expect(total).toBe(20);
  });

  it('should correctly calculate total for multiple items', () => {
    // Тестируем более сложный случай с несколькими товарами
    const items: OrderItem[] = [
      { productId: 'prod-1', quantity: 2, price: 10 },
      { productId: 'prod-2', quantity: 1, price: 20 },
      { productId: 'prod-3', quantity: 3, price: 5 },
    ];

    const total = calculateOrderTotal(items);

    // Проверяем общий расчет: (2 * 10) + (1 * 20) + (3 * 5) = 20 + 20 + 15 = 55
    expect(total).toBe(55);
  });

  it('should return 0 for empty items array', () => {
    // Важно проверить граничный случай - пустой массив товаров
    const total = calculateOrderTotal([]);
    expect(total).toBe(0);
  });
});

// Теперь протестируем функцию createOrder
describe('createOrder', () => {
  it('should create order with correct structure', () => {
    // Создаем мок для генератора ID
    const mockGenerateId = vi.fn().mockReturnValue('test-id-123');

    const items: OrderItem[] = [
      { productId: 'prod-1', quantity: 2, price: 10 },
    ];

    const order = createOrder(items, mockGenerateId);

    // Проверяем структуру и значения созданного заказа
    expect(order).toEqual({
      id: 'test-id-123',
      items: items,
      total: 20, // 2 * 10
    });

    // Проверяем, что генератор ID был вызван
    expect(mockGenerateId).toHaveBeenCalledOnce();
  });

  it('should create order with multiple items and calculate correct total', () => {
    const mockGenerateId = vi.fn().mockReturnValue('test-id-456');

    const items: OrderItem[] = [
      { productId: 'prod-1', quantity: 2, price: 10 },
      { productId: 'prod-2', quantity: 1, price: 20 },
    ];

    const order = createOrder(items, mockGenerateId);

    expect(order).toEqual({
      id: 'test-id-456',
      items: items,
      total: 40, // (2 * 10) + (1 * 20)
    });
  });

  it('should create order with empty items array', () => {
    const mockGenerateId = vi.fn().mockReturnValue('test-id-789');

    const order = createOrder([], mockGenerateId);

    // Проверяем, что заказ создается корректно даже с пустым списком товаров
    expect(order).toEqual({
      id: 'test-id-789',
      items: [],
      total: 0,
    });
  });
});

// Наконец, тестируем функцию processOrder
describe('processOrder', () => {
  it('should successfully process and save order', async () => {
    // Создаем мок хранилища заказов
    const mockStorage: OrderStorage = {
      save: vi.fn().mockResolvedValue(undefined),
    };

    const testOrder: Order = {
      id: 'test-id',
      items: [{ productId: 'prod-1', quantity: 1, price: 10 }],
      total: 10,
    };

    // Выполняем обработку заказа
    await processOrder(testOrder, mockStorage);

    // Проверяем, что заказ был сохранен с правильными данными
    expect(mockStorage.save).toHaveBeenCalledWith(testOrder);
    expect(mockStorage.save).toHaveBeenCalledOnce();
  });

  it('should handle storage errors during order processing', async () => {
    // Создаем мок хранилища, который генерирует ошибку
    const mockStorage: OrderStorage = {
      save: vi.fn().mockRejectedValue(new Error('Storage error')),
    };

    const testOrder: Order = {
      id: 'test-id',
      items: [{ productId: 'prod-1', quantity: 1, price: 10 }],
      total: 10,
    };

    // Проверяем, что ошибка хранилища корректно обрабатывается
    await expect(async () => {
      await processOrder(testOrder, mockStorage);
    }).rejects.toThrowError('Storage error');
  });
});
```

Иногда вместо моков лучше строить зависимости с интерфейсами, а в тестах предоставлять лёгкие, но полноценные реализации. Это подходит для более сложных систем и тестов. Такой подход часто называют **заменой моков на тестовую инфраструктуру**.

**Когда это полезно:** когда код сложный, и поведение моков выходит за пределы контроля. Например, для интеграционных или end-to-end тестов.

Помимо этого, иногда говорят про использование **инвариантных тестов**, если вообще отказываются от изоляции через подмены.

Инвариантные тесты - это тесты, которые проверяют реальное поведение системы без изоляции от внешних зависимостей. Вместо использования моков, фейков или стабов, они работают с реальными компонентами, инфраструктурой и данными. Такой подход применяют, чтобы удостовериться, что вся система работает как ожидается, а изменения в коде не нарушают контракт между компонентами.

### Чек-лист тестируемости

Давайте разберёмся с основными принципами тестируемости, их значением и как это реализовать на практике.

**1. Код легко запускается в изолированном окружении**

Код должен быть независим от конкретной среды, чтобы его можно было легко протестировать без долгой настройки.

**Пример: Устранение зависимости от внешних ресурсов**

Если ваш код напрямую зависит от базы данных или API, это создаёт сложности для тестирования. Чтобы этого избежать, используйте абстракции.

**До (зависимость от базы):**

```tsx
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

export async function getUser(id: number) {
  return await prisma.user.findUnique({ where: { id } });
}
```

**После (внедрение зависимости):**

```tsx
export interface UserRepository {
  findById(id: number): Promise<{ id: number; name: string } | null>;
}

export async function getUser(id: number, repo: UserRepository) {
  return await repo.findById(id);
}
```

**Тест:**

```tsx
import { describe, it, expect } from 'vitest';
import { getUser } from './user';

describe('getUser', () => {
  it('should return a user by id', async () => {
    const mockRepo = {
      findById: async (id: number) => ({ id, name: 'John Doe' }),
    };

    const user = await getUser(1, mockRepo);
    expect(user).toEqual({ id: 1, name: 'John Doe' });
  });
});
```

**2. Минимум побочных эффектов (side effects)**

Код должен быть предсказуемым и управляемым. Побочные эффекты (например, запись в файл, изменение глобальных переменных) затрудняют тестирование.

**Пример: Устранение побочных эффектов**

**До (побочные эффекты):**

```tsx
let userCount = 0;

export function addUser(name: string) {
  userCount++;
  console.log(`Added user: ${name}`);
  return { id: userCount, name };
}
```

**После (без побочных эффектов):**

```tsx
export function addUser(name: string, currentCount: number) {
  return { id: currentCount + 1, name };
}
```

**Тест:**

```tsx
import { describe, it, expect } from 'vitest';
import { addUser } from './user';

describe('addUser', () => {
  it('should return a new user with incremented ID', () => {
    const user = addUser('Jane Doe', 0);
    expect(user).toEqual({ id: 1, name: 'Jane Doe' });
  });
});
```

**3. Возможность замены зависимостей (моки, стабы, фейки)**

Это упрощает изоляцию тестируемого модуля и проверку его логики без необходимости взаимодействия с внешними компонентами.

**Пример: Стабы**

**До (жёсткая зависимость):**

```tsx
import axios from 'axios';

export async function fetchUser(id: number) {
  const response = await axios.get(`/api/users/${id}`);
  return response.data;
}
```

**После (использование интерфейса):**

```tsx
export interface HttpClient {
  get(url: string): Promise<{ data: any }>;
}

export async function fetchUser(id: number, httpClient: HttpClient) {
  const response = await httpClient.get(`/api/users/${id}`);
  return response.data;
}
```

**Тест с использованием стаба:**

```tsx
import { describe, it, expect } from 'vitest';
import { fetchUser } from './user';

describe('fetchUser', () => {
  it('should return user data', async () => {
    const mockHttpClient = {
      get: async (url: string) => ({ data: { id: 1, name: 'John Doe' } }),
    };

    const user = await fetchUser(1, mockHttpClient);
    expect(user).toEqual({ id: 1, name: 'John Doe' });
  });
});
```

**Практический чек-лист для тестируемости:**

• **Логика отделена от инфраструктуры:** проверьте, что ваш код можно запустить без настройки внешней среды.
• **Модули независимы:** компоненты работают независимо друг от друга.
• **Используйте внедрение зависимостей:** например, передавайте интерфейсы для работы с базой данных или API.
• **Минимизируйте глобальное состояние:** вместо изменения глобальных переменных используйте функции с параметрами.
Применяя этот чек-лист, Вы сможете писать тестируемый код, который легко поддерживать и масштабировать.

## Какие бывают тесты? Пирамида тестирования

Пирамида тестирования - это концепция, которая помогает распределить тесты в проекте для достижения высокой надёжности и эффективности. Её цель - минимизировать время и усилия, затрачиваемые на написание и выполнение тестов, при этом обеспечивая покрытие всех важных аспектов системы.

### Уровни пирамиды:

1. **Юнит-тесты (Unit Tests)**:

   Основание пирамиды. Тестируют отдельные модули или функции изолированно. Быстрые, дешёвые, и их должно быть больше всего.

2. **Интеграционные тесты (Integration Tests)**:

   Средний слой. Проверяют взаимодействие между модулями или компонентами. Например, взаимодействие между функцией и базой данных.

3. **End-to-End тесты (E2E Tests)**:

   Верхушка пирамиды. Покрывают весь пользовательский сценарий, от интерфейса до базы данных. Самые медленные и дорогие в поддержке.

## 1. Юнит-тесты

Юнит-тесты проверяют изолированную логику отдельных функций или классов. Они выполняются быстро, требуют минимум ресурсов и облегчают отладку.

**Основные характеристики:**

• **Тестируют отдельные модули или функции.**
• **Изолированы от внешних зависимостей.** Используют моки, стабы или фейки.
• **Самые быстрые и дешёвые в написании.** Не требуют настройки инфраструктуры.
• **Выявляют ошибки на ранних этапах разработки.**

Рассмотрим пример функции для трекера задач, например, добавление задачи в колонку. Такой функционал часто используется в реальных приложениях.

**Условия:**

1. Каждая задача должна иметь уникальный идентификатор.
2. Задача добавляется в указанную колонку, если она существует.
3. Если колонка не найдена, функция должна выбрасывать ошибку.
4. В задаче должны быть обязательные поля: title и status.

**Реализация функции:**

```tsx
export interface Task {
  id: string;
  title: string;
  status: 'todo' | 'in-progress' | 'done';
}

export interface Column {
  id: string;
  name: string;
  tasks: Task[];
}

export function addTaskToColumn(
  columns: Column[],
  columnId: string,
  task: Omit<Task, 'id'>,
  generateId: () => string
): Column[] {
  const column = columns.find((col) => col.id === columnId);
  if (!column) {
    throw new Error(`Column with ID "${columnId}" not found.`);
  }

  const newTask: Task = { ...task, id: generateId() };
  column.tasks.push(newTask);
  return columns;
}
```

**Тестируем сценарии:**

1. Добавление задачи в существующую колонку.
2. Ошибка при попытке добавить задачу в несуществующую колонку.
3. Проверка, что задачи имеют уникальные идентификаторы.
4. Обработка пустого списка колонок.

```tsx
import { describe, it, expect, vi } from 'vitest';
import { addTaskToColumn, Column } from './board';

describe('addTaskToColumn', () => {
  it('should add a task to an existing column', () => {
    const columns: Column[] = [{ id: 'col-1', name: 'To Do', tasks: [] }];
    const task = { title: 'New Task', status: 'todo' };
    const generateId = vi.fn().mockReturnValue('task-1');

    const updatedColumns = addTaskToColumn(columns, 'col-1', task, generateId);

    expect(updatedColumns[0].tasks).toHaveLength(1);
    expect(updatedColumns[0].tasks[0]).toEqual({
      id: 'task-1',
      title: 'New Task',
      status: 'todo',
    });
  });

  it('should throw an error if the column does not exist', () => {
    const columns: Column[] = [];
    const task = { title: 'New Task', status: 'todo' };
    const generateId = vi.fn().mockReturnValue('task-1');

    expect(() => addTaskToColumn(columns, 'col-1', task, generateId)).toThrow(
      'Column with ID "col-1" not found.'
    );
  });

  it('should assign a unique ID to each task', () => {
    const columns: Column[] = [{ id: 'col-1', name: 'To Do', tasks: [] }];
    const task1 = { title: 'Task 1', status: 'todo' };
    const task2 = { title: 'Task 2', status: 'todo' };
    const generateId = vi
      .fn()
      .mockReturnValueOnce('task-1')
      .mockReturnValueOnce('task-2');

    let updatedColumns = addTaskToColumn(columns, 'col-1', task1, generateId);
    updatedColumns = addTaskToColumn(
      updatedColumns,
      'col-1',
      task2,
      generateId
    );

    expect(updatedColumns[0].tasks).toHaveLength(2);
    expect(updatedColumns[0].tasks[0].id).toBe('task-1');
    expect(updatedColumns[0].tasks[1].id).toBe('task-2');
  });

  it('should handle an empty list of columns', () => {
    const columns: Column[] = [];
    const task = { title: 'Task in Empty Board', status: 'todo' };
    const generateId = vi.fn().mockReturnValue('task-1');

    expect(() => addTaskToColumn(columns, 'col-1', task, generateId)).toThrow(
      'Column with ID "col-1" not found.'
    );
  });
});
```

**Когда использовать юнит-тесты:**

• **Бизнес-логика:** Например, расчёт скидок или обработка пользовательских данных.
• **Утилитарные функции:** Например, конвертация данных или форматирование строк.
• **Маленькие модули:** Например, валидация входных данных.

**Преимущества юнит-тестов:**

1. **Быстрое выполнение:** Тысячи юнит-тестов могут выполняться за секунды.
2. **Раннее обнаружение багов:** Легко проверить работу кода, пока он ещё свежий.
3. **Документация:** Каждый тест служит примером использования функции.
4. **Простота:** Юнит-тесты требуют меньше всего настройки.

### 2. Интеграционные тесты (Integration Tests)

Интеграционные тесты проверяют взаимодействие между частями системы, такими как функции, модули, или даже микросервисы. Это следующий уровень после юнит-тестов, направленный на то, чтобы убедиться, что разные части кода корректно работают вместе.

**Основные характеристики интеграционных тестов:**

1. **Тестируют взаимодействие:** Проверяют, как функции, модули или компоненты работают вместе.
2. **Менее изолированные:** Зависимости могут быть реальными (например, базой данных) или подставными (моки, стабы).
3. **Имеют дело с побочными эффектами:** Например, запись данных в базу или вызов API.
4. **Медленнее, чем юнит-тесты, но быстрее, чем E2E.**

**Когда нужны интеграционные тесты?**

• Проверка взаимодействия между модулями: Например, как бэкенд возвращает данные фронтенду.
• Убедиться, что реальные зависимости (база данных, API) работают как ожидается.
• Проверка сложной бизнес-логики, требующей нескольких этапов обработки.

**Пример: Сохранение задачи с записью в базу данных**

**Описание задачи:**

1. У нас есть функцию saveTask, которая:

   • Проверяет корректность данных задачи.
   • Сохраняет задачу в базу данных.
   • Возвращает задачу с уникальным ID, присвоенным базой.

2. Побочные эффекты:

   • Запись данных в базу.
   • Взаимодействие с модулем валидации.

3. Модули:

   • validator: проверяет данные задачи.
   • database: отвечает за взаимодействие с базой данных.

**Реализация функции:**

```tsx
// Модуль для валидации данных задачи
export function validateTask(task: { title: string; status: string }): void {
  if (!task.title) {
    throw new Error('Task title is required.');
  }
  if (!['todo', 'in-progress', 'done'].includes(task.status)) {
    throw new Error('Invalid task status.');
  }
}

// Модуль для работы с базой данных
export interface Database {
  save: (data: any) => { id: string; [key: string]: any };
}

export function saveTask(
  task: { title: string; status: string },
  database: Database
): { id: string; title: string; status: string } {
  validateTask(task); // Взаимодействие с модулем валидации
  return database.save(task); // Взаимодействие с базой данных
}
```

**Тестируем сценарии:**

1. Успешное сохранение задачи.
2. Ошибка при некорректных данных.
3. Проверка взаимодействия с базой данных (мок).

**Тесты:**

```tsx
import { describe, it, expect, vi } from 'vitest';
import { saveTask, validateTask, Database } from './task';

describe('saveTask', () => {
  it('should save a valid task to the database', () => {
    const mockDatabase: Database = {
      save: vi
        .fn()
        .mockReturnValue({ id: 'task-123', title: 'New Task', status: 'todo' }),
    };

    const task = { title: 'New Task', status: 'todo' };

    const result = saveTask(task, mockDatabase);

    expect(mockDatabase.save).toHaveBeenCalledOnce();
    expect(mockDatabase.save).toHaveBeenCalledWith(task);
    expect(result).toEqual({
      id: 'task-123',
      title: 'New Task',
      status: 'todo',
    });
  });

  it('should throw an error if task data is invalid', () => {
    const mockDatabase: Database = { save: vi.fn() };

    const invalidTask = { title: '', status: 'unknown' };

    expect(() => saveTask(invalidTask, mockDatabase)).toThrow(
      'Task title is required.'
    );
    expect(mockDatabase.save).not.toHaveBeenCalled();
  });

  it('should ensure database save method is called once', () => {
    const mockDatabase: Database = {
      save: vi
        .fn()
        .mockReturnValue({ id: 'task-123', title: 'New Task', status: 'todo' }),
    };

    const task = { title: 'New Task', status: 'todo' };

    saveTask(task, mockDatabase);

    expect(mockDatabase.save).toHaveBeenCalledTimes(1);
  });
});
```

**Почему это интеграционный тест?**

1. **Взаимодействие между модулями:**

   • Функция взаимодействует с модулем валидации (validateTask) и базой данных (Database).
   • Оба модуля работают вместе для достижения результата.

2. **Побочные эффекты:**

   • Вызов функции [database.save](http://database.save) - это побочный эффект, симулирующий запись данных.
   • Тест проверяет, что запись произошла корректно.

3. **Сложность логики:**

   • Функция объединяет логику валидации и сохранения.
   • Учитываются сценарии ошибок и корректного выполнения.

Этот пример показывает, как тестировать функцию, объединяющую несколько модулей и вызывающую побочные эффекты.

Такие тесты полезны, чтобы убедиться, что взаимодействие между разными частями системы работает правильно.

**Основные случаи применения:**

1. **Проверка взаимодействия модулей:**

   • Например, убедиться, что компонент фронтенда корректно вызывает API и обрабатывает ответ.

2. **Тестирование бизнес-логики, зависящей от нескольких модулей:**

   • Например, функция, которая сохраняет данные в базу после их валидации.

3. **Работа с внешними сервисами или базами данных:**

   • Например, отправка данных в систему платежей и проверка ответа.

4. **Проверка побочных эффектов:**

   • Например, что отправка email происходит только после успешного сохранения данных.

5. **Промежуточное звено между юнит- и E2E-тестами:**

   • Когда необходимо протестировать сложную логику или взаимодействие без запуска всей системы.

**Преимущества интеграционных тестов:**

1. **Реалистичная проверка взаимодействий:**

   • Убедиться, что модули работают вместе так, как ожидается.

2. **Выявление проблем интеграции:**

   • Например, несоответствие контрактов между модулями (фронтенд ожидает одно поле, а бэкенд возвращает другое).

3. **Менее затратные по сравнению с E2E:**

   • Интеграционные тесты требуют меньше ресурсов, чем полноценное тестирование всей системы.

4. **Отражают реальное поведение системы:**

   • Тестируют побочные эффекты, которые важны для конечного результата.

5. **Быстрое нахождение ошибок:**

   • Упрощают поиск проблем в интеграции, не требуя разворачивания полной инфраструктуры.

**Недостатки интеграционных тестов:**

1. **Сложность настройки окружения:**

   • Например, нужно настроить тестовую базу данных, моки внешних сервисов.

2. **Более высокая стоимость выполнения:**

   • Они медленнее, чем юнит-тесты, так как затрагивают больше компонентов.

3. **Сложность локализации ошибок:**

   • Если тест падает, причина может быть в любом из компонентов, участвующих в интеграции.

4. **Часто зависят от внешних факторов:**

   • Например, от доступности API или работоспособности базы данных, если не используются моки.

5. **Меньшая изоляция:**

   • Изменения в одной части системы могут вызывать нестабильность тестов, особенно если они плохо спроектированы.

**Когда отдавать предпочтение интеграционным тестам?**

Используйте интеграционные тесты, если:

• Вы хотите протестировать конкретное взаимодействие между двумя или несколькими компонентами.
• Ваша логика включает несколько этапов, и юнит-тесты не могут охватить всё поведение.
• Вы ищете баланс между скоростью выполнения и реалистичностью, не прибегая к E2E тестам.
Интеграционные тесты - это отличный инструмент для выявления проблем на уровне взаимодействия и ускорения разработки, если они используются правильно.

## 3. E2E-тесты

E2E (end-to-end) тесты проверяют пользовательские сценарии - от взаимодействия с интерфейсом до выполнения операций на сервере и записи данных в базу. Они наиболее приближены к реальному поведению системы и позволяют убедиться, что все её части работают вместе, как ожидается.

**Что такое E2E тестирование?**

1. **Полный пользовательский сценарий:**

   • Тест начинается с действий пользователя в интерфейсе и проходит через все уровни приложения до конечного результата.

1. **Проверка “сквозной” работы системы:**

   • Включает фронтенд, API, базы данных и любые другие интеграции.

1. **Используемые инструменты:**

   • Для тестирования интерфейса популярны инструменты, такие как Playwright, Cypress, Puppeteer.

**Пример: Сценарий аутентификации пользователя:**

**Что будем тестировать?**

1. Пользователь заходит на страницу логина.
2. Вводит email и пароль.
3. Нажимает кнопку «Войти».
4. Успешно переходит на защищённую страницу после авторизации.

**Что проверим?**

• Полный процесс от ввода данных до авторизации.
• Корректную работу валидации на клиенте.
• Взаимодействие фронтенда с API.
• Отображение ошибок при неправильных данных.

**Код теста:**

```tsx
import { test, expect } from '@playwright/test';

test.describe('User Authentication', () => {
  test('should successfully log in with valid credentials', async ({
    page,
  }) => {
    // Открываем страницу логина
    await page.goto('http://localhost:3000/login');

    // Проверяем, что находимся на странице логина
    await expect(page).toHaveURL(/\/login/);
    await expect(page.locator('h1')).toHaveText('Login');

    // Вводим email и пароль
    await page.fill('input[name="email"]', 'user@example.com');
    await page.fill('input[name="password"]', 'password123');

    // Нажимаем кнопку "Войти"
    await page.click('button[type="submit"]');

    // Ожидаем переход на защищённую страницу
    await expect(page).toHaveURL(/\/dashboard/);
    await expect(page.locator('h1')).toHaveText('Welcome, User!');
  });

  test('should show an error for invalid credentials', async ({ page }) => {
    // Открываем страницу логина
    await page.goto('http://localhost:3000/login');

    // Вводим неправильные email и пароль
    await page.fill('input[name="email"]', 'wrong@example.com');
    await page.fill('input[name="password"]', 'wrongpassword');

    // Нажимаем кнопку "Войти"
    await page.click('button[type="submit"]');

    // Проверяем отображение ошибки
    const errorMessage = page.locator('.error-message');
    await expect(errorMessage).toHaveText('Invalid email or password');
    await expect(page).toHaveURL(/\/login/);
  });
});
```

**Что здесь происходит:**

1. test.describe**:**

   • Группирует тесты, относящиеся к одной функциональности (в данном случае аутентификация).

2. **Позитивный сценарий:**

   • Проверяем успешный вход с корректными данными и переход на защищённую страницу.

3. **Негативный сценарий:**

   • Проверяем отображение ошибки при неверных данных.

4. **Playwright API:**

   • page.goto - открывает URL.
   • page.fill - заполняет поля формы.
   • [page.click](http://page.click) - имитирует нажатие кнопки.
   • expect - проверяет ожидаемые результаты.

**Почему E2E тесты важны?**

1. **Тестирование как пользователь:**

   • Покрывает весь путь, который проходит пользователь, включая взаимодействие с интерфейсом.

2. **Раннее выявление ошибок:**

   • Убеждаемся, что интеграция всех частей системы работает корректно.

3. **Максимальная реалистичность:**

   • Проверяем приложение в условиях, близких к реальным.

**Преимущества и недостатки E2E тестов:**

**Преимущества:**

• Полная проверка пользовательского сценария.
• Минимизируют риск того, что система не работает в реальном использовании.
• Выявляют проблемы интеграции.

**Недостатки:**

• Медленные: выполнение E2E тестов занимает больше времени, чем юнит- или интеграционных тестов.
• Сложны в настройке: требуют развёрнутой среды (сервер, база данных).
• Дорогие: изменения в системе могут потребовать переписывания тестов.

E2E тесты незаменимы для проверки полноты и качества пользовательских сценариев. Они дополняют юнит- и интеграционные тесты, позволяя убедиться, что приложение работает как целостная система.

## Чем больше тестов, тем лучше?

Суть пирамиды тестирования в том, чтобы не перегружать систему сложными и медленными тестами на верхних уровнях (E2E), а вместо этого фокусироваться на большом количестве быстрых и простых юнит-тестов, которые дают высокое покрытие и быстро обнаруживают ошибки. Интеграционные тесты составляют связку между юнитами и полными сценариями, проверяя основные взаимодействия и обеспечивая стабильность на уровне более сложных логик.

**Идеальное распределение:**

• **Юнит-тесты:** 70-80% всех тестов.
• **Интеграционные тесты:** 15-20% всех тестов.
• **E2E тесты:** 5-10% всех тестов.

Но не забывайте, дело не только в количестве, но и в качестве. Даже 100% покрытие тестами не всегда гарантирует полную работоспособность приложения, если они тестируют, например, имплементацию, а не поведение.

## Когда запускать тесты?

Разные типы тестов имеют свои цели и оптимальное время для запуска, чтобы обеспечить баланс между скоростью разработки и стабильностью проекта.

### Юнит-тесты

**Когда запускать:** при каждом сохранении кода (локально).

**Цели:**

- Быстро проверять мелкие изменения в логике.
- Давать разработчику мгновенную обратную связь.

**Как запускать:** используйте утилиты вроде vitest с флагом --watch, чтобы тесты запускались автоматически при изменении файлов.

**Пример:** проверка функции фильтрации задач при изменении логики.

### Интеграционные тесты

**Когда запускать:** на этапе CI/CD.

**Цели:**

- Убедиться, что отдельные модули корректно работают вместе.
- Предотвратить поломки на уровне взаимодействия компонентов.

**Как запускать:** интеграционные тесты запускаются автоматически на сервере CI (например, GitHub Actions, GitLab CI). Запускайте перед созданием сборок для тестовых или production-окружений.

**Пример:** проверка связи фронтенда с API при обновлении endpoints.

### E2E тесты

**Когда запускать:** перед релизом, в ночных сборках или в тестовом окружении.

**Цели:**

- Проверить полный пользовательский сценарий от начала до конца.
- Убедиться, что приложение работает в реальных условиях.

**Как запускать:** используйте инструменты вроде Playwright или Cypress для автоматического выполнения тестов. Настройте запуск в изолированном окружении с фиктивными данными.

**Пример:** тестирование регистрации, авторизации и управления задачами в интерфейсе приложения.

### Советы по запуску тестов

1. **Автоматизация:** Настройте CI/CD так, чтобы тесты запускались без ручного вмешательства.
2. **Приоритет:** Запускайте быстрые тесты (юнит) локально, а более дорогие (E2E) - на сервере.
3. **Разделение ответственности:** Убедитесь, что каждый тип теста проверяет свою область (юнит - логику, интеграционные - взаимодействие, E2E - пользовательский сценарий).

## Тестирование приложений

Когда речь идет о тестировании, подходы зависят от типа проекта. Приложения и библиотеки имеют разные цели и архитектуру, что напрямую влияет на стратегию тестирования.

Приложения ориентированы на конечного пользователя, поэтому основное внимание уделяется стабильности функций и пользовательскому опыту.

### Юнит-тесты

Проверяют логику отдельных функций или компонентов.

**Пример:** проверка работы функции фильтрации задач по дате.
**Цель:** обеспечить корректность базовой логики.

### Интеграционные тесты

Проверяют взаимодействие между различными модулями (например, фронтендом и API).

**Пример:** проверка, что фронтенд корректно отображает задачи, полученные от API.
**Цель:** убедиться, что отдельные части приложения работают вместе.

### E2E тесты

Проверяют полный пользовательский сценарий - от интерфейса до базы данных.

**Пример:** авторизация, создание задач в трекере, добавление товаров в корзину.
**Цель:** убедиться, что пользовательский опыт работает без ошибок.

### Особенности тестирования приложений

**Юнит-тесты** используются для проверки бизнес-логики и утилитарных функций. Основной акцент делается на **интеграционные** и **E2E тесты**, так как важно удостовериться, что все уровни приложения работают вместе.

## Тестирование библиотек

Библиотеки предназначены для повторного использования, поэтому тесты должны обеспечивать надежность API, стабильность в различных окружениях и поддержку разных конфигураций.

### Юнит-тесты

Проверяют отдельные функции и методы, составляющие API библиотеки.

**Пример:** тестирование функции сортировки массива.

### Тесты на конфигурации

Проверяют работу библиотеки с различными настройками.

**Пример:** убедиться, что библиотека корректно работает с разными параметрами.

### Интеграционные тесты

Проверяют работу библиотеки в связке с популярными инструментами.

**Пример:** проверка работы библиотеки с фреймворками React и Vue.

### Особенности тестирования библиотек

Основной акцент делается на **юнит-тесты**, так как важно, чтобы каждый элемент API работал корректно. **Интеграционные тесты** важны для проверки совместимости с популярными инструментами. **E2E тесты** редко применяются, так как библиотеки, как правило, не имеют пользовательского интерфейса.

## Заключение

Таким образом, тесты - это не просто способ проверки корректности кода, а мощный инструмент, который помогает разработчикам создавать качественную архитектуру и поддерживать её в хорошем состоянии на протяжении всего жизненного цикла проекта. Они не только выявляют существующие ошибки, но и помогают предотвратить их появление в будущем, направляя разработчиков к лучшим архитектурным решениям.
