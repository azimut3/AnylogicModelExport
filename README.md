# AnylogicModelExport

Для того чтобы совершить прогон подели, необходимо добавить следующие библиотеки в зависимости проекта:

- com.xj.anylogic.engine.al3d.jar
- com.xj.anylogic.engine.jar
- com.xj.anylogic.engine.nl.jar
- com.xj.anylogic.engine.sa.jar
- model.jar

После добавления в зависимости проекта необходимо создать экземпляр эксперимента и назначить параметры эксперимента для прогона.

```

CustomExperiment customExperiment = new CustomExperiment(null);
customExperiment.varOfWork = 3;
customExperiment.capacityOfMainConveyor = 1200;
customExperiment.quantityOfVagonsToSilageAtOnce = 8;
customExperiment.quantityOfVehicleDischargeStations = 4;
customExperiment.numberOfVehicleSilages = 4;
customExperiment.capacityOfVehicleSilage = 1000;
customExperiment.quantityOfSilages = 19;
customExperiment.yearsModelWorking = 8;

customExperiment.run();

```

Следующе параметры могут варьироваться в следующих пределах:
- varOfWork:
значение всегда должно быть равно 3;
- capacityOfMainConveyor:
значение может варьироваться в пределах 800-1200 тонн;
- quantityOfVagonsToSilageAtOnce:
значение может варьироваться в пределах 8-10 вагонов;
- quantityOfVehicleDischargeStations:
значение может варьироваться в пределах 1-4 станций;
- capacityOfVehicleSilage:
значение может варьироваться в пределах 800-1000 тонн;
- quantityOfSilages:
значение может варьироваться в пределах 17-19 силосов;
//этот параметр позволяет существенно ускорять время прогона модели. Чем больше лет указано для прогона модели, тем дольше идет прогон.
- yearsModelWorking:
значение может варьироваться в пределах 1-8 лет;

Перечисленные выше параметры можно варьировать и в других пределах, однако ранее это не тестировалось. В теории это возможно.

После прогона мы получаем объект содержащий статистику о прогоне:

```
IterationStats iterationStats = runExperiment(iteration);

//количество обработаных судов
iterationStats.physicalStats.shipmentStats.vesselsHandledQtt,

//Себестоимость перевалки
iterationStats.economicStats.primeCost

//Медиана времени обратки судна у причала
iterationStats.physicalStats.shipmentStats.medianHandlingTime

//Медиана времени обработки заявки терминалом в целом
iterationStats.physicalStats.shipmentStats.medianAtTerminalTime

//общая прибыль без вычета расзходов
iterationStats.economicStats.income

// Все расходы по эксплуатации терминала
iterationStats.economicStats.costs.total
```


# Основные пареметры которые представляют интерес:

- чистая прибыль:

(iterationStats.economicStats.income - iterationStats.economicStats.costs.total)

- себестоимость

iterationStats.economicStats.primeCost

- время ожидания обработки судном

(iterationStats.physicalStats.shipmentStats.medianHandlingTime - iterationStats.physicalStats.shipmentStats.medianAtTerminalTime)

- общее время обработки заявки

(iterationStats.physicalStats.shipmentStats.medianHandlingTime - iterationStats.physicalStats.shipmentStats.medianAtTerminalTime)

# Examples of generated data and pareto borders
![processingTime_to_annualProffit](https://user-images.githubusercontent.com/56626185/145488299-6bd9a127-91d9-46d5-af25-673d7939404b.jpeg)
![processingTime_to_transhipment cost](https://user-images.githubusercontent.com/56626185/145488303-f9c00e0a-0d6d-4844-8683-21605366cd83.jpeg)
