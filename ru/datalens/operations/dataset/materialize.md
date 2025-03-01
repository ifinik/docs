# Материализация датасета

{% include [datalens-materialization-off-unavailable](../../../_includes/datalens/datalens-materialization-off-unavailable.md) %}

Чтобы материализовать датасет:

1. На странице навигации найдите датасет и откройте его.
1. В верхней части экрана нажмите значок ![image](../../../_assets/datalens/horizontal-ellipsis.svg) и выберите ![image](../../../_assets/datalens/materialize.svg) **Материализация**.
1. В появившемся окне выберите:

   * **Прямой доступ** — все запросы к данным исполняются на стороне источника. Данные не загружаются в БД материализации.
   * **Единовременная** — данные загружаются в БД материализации единовременно.
   * **Периодическая** — данные загружаются в БД материализации по расписанию.
    
1. Укажите настройки материализации и нажмите **Сохранить**.


#### См. также {#see-also}
- [{#T}](../../concepts/dataset/settings.md#mode)
