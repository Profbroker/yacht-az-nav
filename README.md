# yacht-az-nav

Главное меню сайта [yacht.az](https://yacht.az) — подгружается через jsDelivr CDN в Tilda Header HTML.

## Что внутри

- **`tilda-nav.html`** — полный код меню (HTML + CSS + JS). ~500 строк.
  - Multilang RU/EN/AZ (detect по `window.location.pathname`)
  - Логотип + 7 пунктов меню (2 с dropdown)
  - Соцссылки (Instagram, Telegram) + переключатель языков
  - Бургер-меню для мобилки (полноэкранный overlay)

## Архитектура

```
GitHub репо (Profbroker/yacht-az-nav)
       ↓ push
jsDelivr CDN (cdn.jsdelivr.net/gh/Profbroker/yacht-az-nav/tilda-nav.html)
       ↓ fetch
Tilda Site Settings → Custom Headers → HTML-код в HEAD
       ↓
innerHTML в #yz-nav-mount
       ↓
клиент видит меню
```

## CDN URL

```
https://cdn.jsdelivr.net/gh/Profbroker/yacht-az-nav/tilda-nav.html
```

## Обновление меню

1. Редактировать `tilda-nav.html` (этот репо)
2. Commit + push в main
3. Через ~10 минут обновление видно на yacht.az
4. **Ускорить purge** edge-кэша jsDelivr:
   ```bash
   curl -X POST https://purge.jsdelivr.net/ \
     -H "Content-Type: application/json" \
     -d '{"path":["/gh/Profbroker/yacht-az-nav/tilda-nav.html"]}'
   ```
   Или через UI: <https://www.jsdelivr.com/tools/purge>

## TODO до прода

- [ ] Заменить `LOGO_URL_HERE` на реальный URL логотипа из tildacdn
- [ ] Проверить Instagram URL: <https://instagram.com/yacht.az>
- [ ] Проверить Telegram URL: <https://t.me/yachtaz>
- [ ] Проверить наличие страниц `/lang/shop` и `/lang/blog` (если нет — убрать из меню)

## Установка в Tilda

См. `tilda-nav-loader.html` в основном проекте `yacht-az/webdesign/cdn-blocks/` —
этот файл вставляется в Tilda Site Settings → Custom Headers/Footers → HTML-код
в HEAD. Один раз — работает на всех страницах сайта.

## Безопасность

- Репо публичный, но только владелец (Profbroker) может пушить
- 2FA на GitHub обязательно
- jsDelivr автоматически кэширует — никаких внешних зависимостей в коде нет
