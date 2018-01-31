---
title: "Авторизация в Angular"
---
Проверка прав и доступов на клиенте<!--more-->

> Основная и финальная проверка прав должна быть на сервере!!!
>
> Проверка прав на клиенте делается только для удобства пользователя.


# Что использовать
Для организации проверки прав на клиенте удобно использовать

*[ngx-permissions](https://github.com/AlexKhymenko/ngx-permissions)*



# Как подключить
**pakage.json** (добавляем зависимость)
```javascript
...
"ngx-permissions": "^3.0.0",
...
```

**app.component.ts**
``` javascript
...
// инжектим сервис предоставляемый фреймворком
constructor(private permissionsService: NgxPermissionsService,
...) {
...
}
 
...
ngOnInit(): void {
// authService.getPermissions() - какой-то ваш сервисный метод, например rest запрос на backend
    this.authService.getPermissions().subscribe(permissions =>{
// загружаем масив разрешений, актуальный для данного пользователя
        this.permissionsService.loadPermissions(permissions);
    });
}
```


# Как использовать
## Routing (скрытие навигационных маршрутов)

**app.routing.ts**
``` javascript

export const ROUTES: Routes = [
...
    {
        path: '',
        redirectTo: 'request',
        pathMatch: 'full',
        canActivate: [NgxPermissionsGuard],
        data: {
            permissions: {
                only: ['viewRequests']
            }
        }
    },
...
```

## Скрытие частей страниц
***.html**
``` html

<dx-button
    text="Создать платежное поручение"
    (click)="createRequest101()"
    type="default"
    *ngxPermissionsOnly="['create101']"
></dx-button>
  
<div *ngxPermissionsOnly="['can-see-secret']">
  Секретный текст
</div>
```