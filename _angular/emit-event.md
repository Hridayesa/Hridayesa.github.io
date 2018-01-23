---
title: "Генерация своих событий на компоненте (Angular)"
---
How to. Пример, как сделать публикацию событий на компоненте при помощи EventEmitter<!--more-->

## Публикация события на стороне компонента
``` javascript
@Component({
    selector: 'my-comp',
    ...})
export class MyComponent implements OnInit{
    @Output() onChange = new EventEmitter<string>();
...
    public myFunction() {
        // event emittion
        this.onChange.emit('QQQ');
    }
...
```

## Использование события

``` html
<my-comp (onChange) = 'myEventProcess($event)'></my-comp>
```

``` javascript
...
export class AnotherComponent implements OnInit{
...
    public myEventProcess(event: string) {
        // Do something
    }
...
```
