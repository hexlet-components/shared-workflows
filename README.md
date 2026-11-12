# Общие workflow

Переиспользуемые workflow для репозиториев организаций Хекслета на GitHub. Репозиторий публичный, поэтому его вызывают репозитории любой организации.

## Автослияние Dependabot

`dependabot-automerge.yml` ставит автослияние squash на PR от Dependabot с обновлением minor или patch. Major, неизвестный тип обновления и PR других авторов остаются человеку.

Слияние ждёт обязательных проверок защиты базовой ветки, поэтому до подключения в репозитории включаются две настройки. Первая это Allow auto-merge. Вторая это защита ветки или ruleset с обязательными проверками, имена которых взяты из CI репозитория. Без обязательных проверок `gh pr merge --auto` сливает PR сразу, даже красный, и workflow в таком репозитории ничего не делает.

Репозиторий подключает workflow файлом `.github/workflows/dependabot-automerge.yml`.

```yaml
name: Dependabot auto-merge

on: pull_request

permissions: {}

jobs:
  automerge:
    if: github.event.pull_request.user.login == 'dependabot[bot]'
    permissions:
      contents: write
      pull-requests: write
    uses: hexlet-components/shared-workflows/.github/workflows/dependabot-automerge.yml@7b84d02d32ddeb634561cae27bdb8e4d77a43867 # v1.0.0
```

Вызов закрепляется по SHA тега с версией в комментарии. Линтер `jactionlint` с правилом `unpinned-uses` отвергает `@main`, а такое закрепление Dependabot поднимает сам после нового тега.

Ruleset страховочный шаг пока не видит, он читает только классическую защиту ветки. В репозитории, где обязательные проверки заданы ruleset, workflow пропустит слияние.
