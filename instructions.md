# Install mogp_emulator
```
pip install mogp_emulator=0.5.0
```
(already done if you're using binder)

# Find path of mogp_emulator
```
pip show mogp_emulator
```
Copy and paste the path as the mogp_dir variable in the Experiments/ARTICLE_HM-TuningILC notebook. 

# Install EXETER_MOGP
Execute this command in the main folder: 
```
git clone -b devel https://github.com/BayesExeter/ExeterUQ_MOGP
```

# Modify BuildEmulator.R
Go to ExeterUQ_MOGP/BuildEmulator/BuildEmulator.R. Replace the ```source``` lines by :
```
source("ExeterUQ_MOGP/BuildEmulator/AutoLMcode.R")
source("ExeterUQ_MOGP/BuildEmulator/CustomPredict.R")
```

# Modify HistoryMatching.R
Go to ExeterUQ_MOGP/HistoryMatching/HistoryMatching.R. Replace the source line by:
```
source("ExeterUQ_MOGP/HistoryMatching/impLayoutplot.R")
```
Replace ImplausibilityMOGP function by (to add parallelization, PR request pending):

```
ImplausibilityMOGP <- function(NewData, Emulator, Discrepancy, Obs, ObsErr, parallel=TRUE){
  tEmulator <- Emulator$mogp$predict(as.matrix(NewData), deriv=FALSE)
  if(parallel){
    plan(multicore) ## Run in parallel on local computer
    future_sapply(1:Emulator$mogp$n_emulators, function(k) abs(Obs[k] - tEmulator$mean[k,])/sqrt(tEmulator$unc[k,] + Discrepancy[k] + ObsErr[k]), future.seed=NULL)
  }
  else{
    sapply(1:Emulator$mogp$n_emulators, function(k) abs(Obs[k] - tEmulator$mean[k,])/sqrt(tEmulator$unc[k,] + Discrepancy[k] + ObsErr[k]))
  }
}
```
Same, replace CreateImpList function by

```
CreateImpList <- function(whichVars, VarNames, ImpData, nEms=1, Resolution=c(15,15), whichMax=3,Cutoff=3, parallel=TRUE){
  combGrid <- expand.grid(whichVars[-length(whichVars)],whichVars[-1])
  badRows <- c()
  for(i in 1:length(combGrid[,1])){
    if(combGrid[i,1] >= combGrid[i,2])
      badRows <- c(badRows,i)
  }
    if (!is.null(badRows)) { 
      combGrid <- combGrid[-badRows,]
    }
  combGrid <- combGrid[do.call(order,combGrid),]
  gridList <- lapply(whichVars[-length(whichVars)], function(k) combGrid[which(combGrid[,1]==k),])
  if(parallel){
    future_lapply(gridList, function(e) lapply(1:length(e[,1]), function(k) 
    ImpDensityPanel(x1var=VarNames[e[k,1]], x2var=VarNames[e[k,2]], 
                    ImpData = ImpData, nEms=nEms, Resolution=Resolution, 
                    whichMax = whichMax,Cutoff=Cutoff)))
  }
  else{
    lapply(gridList, function(e) lapply(1:length(e[,1]), function(k) 
    ImpDensityPanel(x1var=VarNames[e[k,1]], x2var=VarNames[e[k,2]], 
                    ImpData = ImpData, nEms=nEms, Resolution=Resolution, 
                    whichMax = whichMax,Cutoff=Cutoff)))
  }
  
}
```


# Please add the utils.R file to the BuildEmulator folder in ExeterUQ_MOGP
