# Git Flow для проекта

## 📋 Обзор

Данный документ описывает процесс работы с ветками в Git для нашего проекта. Мы используем упрощенную версию Git Flow, адаптированную для небольшой команды.

## 🌳 Структура веток

### Основные ветки

#### `main` (production)
- **Назначение**: Стабильная версия продукта в продакшене
- **Кто может пушить**: Только через Pull Request после code review
- **Защита**: Ветка защищена от прямых push'ей

#### `develop` 
- **Назначение**: Интеграционная ветка для новых функций
- **Кто может пушить**: Только через Pull Request
- **Откуда создается**: От `main` при инициализации проекта

### Вспомогательные ветки

#### `feature/*`
- **Назначение**: Разработка новых функций
- **Именование**: `feature/название-функции` (например: `feature/user-authentication`)
- **Откуда создается**: От `develop`
- **Куда вливается**: В `develop`

#### `bugfix/*`
- **Назначение**: Исправление багов в develop
- **Именование**: `bugfix/описание-бага` (например: `bugfix/login-validation-error`)
- **Откуда создается**: От `develop`
- **Куда вливается**: В `develop`

#### `hotfix/*`
- **Назначение**: Срочные исправления в продакшене
- **Именование**: `hotfix/критичный-баг` (например: `hotfix/payment-processing-error`)
- **Откуда создается**: От `main`
- **Куда вливается**: В `main` и `develop`

#### `release/*`
- **Назначение**: Подготовка новой версии к релизу
- **Именование**: `release/версия` (например: `release/1.2.0`)
- **Откуда создается**: От `develop`
- **Куда вливается**: В `main` и `develop`

## 📝 Правила именования веток

```
feature/short-description-in-kebab-case
bugfix/issue-number-short-description
hotfix/critical-issue-description
release/x.y.z
```

### Примеры хороших названий:
- ✅ `feature/add-user-profile`
- ✅ `bugfix/123-fix-login-redirect`
- ✅ `hotfix/payment-gateway-timeout`
- ✅ `release/2.1.0`

### Примеры плохих названий:
- ❌ `feature/NewFeature`
- ❌ `bugfix/баг-авторизации`
- ❌ `fix`
- ❌ `johns-branch`

## 🔄 Процессы работы

### 1. Разработка новой функции

```bash
# 1. Создаем ветку от develop
git checkout develop
git pull origin develop
git checkout -b feature/awesome-feature

# 2. Работаем над функцией
git add .
git commit -m "feat: add awesome feature"

# 3. Пушим изменения
git push origin feature/awesome-feature

# 4. Создаем Pull Request в develop
# После одобрения и merge удаляем ветку
```

### 2. Исправление бага

```bash
# 1. Создаем ветку от develop
git checkout develop
git pull origin develop
git checkout -b bugfix/fix-user-validation

# 2. Исправляем баг
git add .
git commit -m "fix: resolve user validation issue"

# 3. Пушим и создаем PR
git push origin bugfix/fix-user-validation
```

### 3. Срочное исправление (Hotfix)

```bash
# 1. Создаем ветку от main
git checkout main
git pull origin main
git checkout -b hotfix/critical-security-fix

# 2. Исправляем проблему
git add .
git commit -m "hotfix: patch security vulnerability"

# 3. Пушим изменения
git push origin hotfix/critical-security-fix

# 4. Создаем PR в main
# После merge в main, также создаем PR в develop
```

### 4. Подготовка релиза

```bash
# 1. Создаем ветку от develop
git checkout develop
git pull origin develop
git checkout -b release/1.2.0

# 2. Обновляем версию, changelog, финальные правки
git add .
git commit -m "chore: prepare release 1.2.0"

# 3. После тестирования создаем PR в main
# После merge в main, создаем tag
git tag -a v1.2.0 -m "Release version 1.2.0"
git push origin v1.2.0

# 4. Также merge обратно в develop
```

## 📊 Визуальная схема

```
main     ────●────────────●────────────●────────
              ↑            ↑            ↑
              │            │            │
hotfix   ─────●────●───    │            │
              ↓    ↓       │            │
develop  ──●──●────●───●───●────●───●───●────
           ↑       ↑   ↓        ↑   ↓
feature    └───●───┘   └────●───┘   │
                                    │
release                             └───●───●───
```

## ✅ Чек-лист перед созданием Pull Request

- [ ] Код соответствует стандартам проекта
- [ ] Написаны/обновлены тесты
- [ ] Обновлена документация (если требуется)
- [ ] Нет конфликтов с целевой веткой
- [ ] PR имеет понятное описание
- [ ] Указаны связанные issues (если есть)

## 🚀 Команды для быстрого старта

### Начало работы над новой задачей
```bash
# Обновляем develop
git checkout develop && git pull

# Создаем feature ветку
git checkout -b feature/my-feature

# Или bugfix ветку
git checkout -b bugfix/my-bugfix
```

### Завершение работы над задачей
```bash
# Коммитим изменения
git add .
git commit -m "feat: description of changes"

# Пушим ветку
git push origin feature/my-feature

# Создаем PR через GitHub/GitLab/Bitbucket
```

### Синхронизация с основной веткой
```bash
# Находясь в своей feature ветке
git checkout develop
git pull origin develop
git checkout feature/my-feature
git merge develop
```

## 📌 Дополнительные правила

1. **Никогда** не пушьте напрямую в `main` или `develop`
2. **Всегда** используйте Pull Requests для code review
3. **Удаляйте** ветки после merge
4. **Регулярно** синхронизируйтесь с develop во время работы
5. **Используйте** осмысленные сообщения коммитов

## 🔖 Соглашение о коммитах

Используем conventional commits:
- `feat:` - новая функциональность
- `fix:` - исправление бага
- `docs:` - изменения в документации
- `style:` - форматирование кода
- `refactor:` - рефакторинг кода
- `test:` - добавление тестов
- `chore:` - обновление зависимостей и прочее

### Примеры:
```bash
git commit -m "feat: add user registration"
git commit -m "fix: resolve memory leak in data processing"
git commit -m "docs: update API documentation"
```

## 🆘 Решение проблем

### Случайно запушил в main
```bash
# Создаем новую ветку с изменениями
git checkout -b feature/my-changes

# Возвращаем main к предыдущему состоянию
git checkout main
git reset --hard origin/main
git push --force-with-lease
```

### Конфликты при merge
```bash
# Обновляем целевую ветку
git checkout develop
git pull origin develop

# Возвращаемся в свою ветку и merge
git checkout feature/my-feature
git merge develop

# Разрешаем конфликты вручную
# После разрешения:
git add .
git commit -m "merge: resolve conflicts with develop"
```

## 📚 Полезные ссылки

- [Git Flow оригинальная модель](https://nvie.com/posts/a-successful-git-branching-model/)
- [GitHub Flow](https://guides.github.com/introduction/flow/)
- [Conventional Commits](https://www.conventionalcommits.org/)
