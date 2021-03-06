[![Go](https://github.com/MandelV/randomForest/actions/workflows/go.yml/badge.svg?branch=master)](https://github.com/MandelV/randomForest/actions/workflows/go.yml)


GoDoc: https://godoc.org/github.com/malaschitz/randomForest

This fork add Saving/Loading functions see the section Saving/Loading [click on this link](##-Saving/Loading)

Test: 
```go
go test ./... -cover -coverpkg=.  
```

# randomForest
[Random Forest](https://en.wikipedia.org/wiki/Random_forest) implementation in golang. 

## Simple Random Forest

```go
	xData := [][]float64{}
	yData := []int{}
	for i := 0; i < 1000; i++ {
		x := []float64{rand.Float64(), rand.Float64(), rand.Float64(), rand.Float64()}
		y := int(x[0] + x[1] + x[2] + x[3])
		xData = append(xData, x)
		yData = append(yData, y)
	}
	forest := randomForest.Forest{}
	forestData := randomForest.ForestData{X: xData, Class: yData}
	forest.Data = forestData
	forest.Train(1000)
	//test
	fmt.Println("Vote", forest.Vote([]float64{0.1, 0.1, 0.1, 0.1})) 
	fmt.Println("Vote", forest.Vote([]float64{0.9, 0.9, 0.9, 0.9}))
```

## Extremely Randomized Trees

```go
	forest.TrainX(1000)	
```

## Deep Forest

Deep forest inspired by https://arxiv.org/abs/1705.07366

```go
    dForest := forest.BuildDeepForest()
    dForest.Train(20, 100, 1000) //20 small forest with 100 trees help to build deep forest with 1000 trees
```

## Continuos Random Forest

Continuos Random Forest for data where are still new and new data (forex, wheather, user logs, ...). New data create a new trees and oldest trees are removed.

```go
forest := randomForest.Forest{}
data := []float64{rand.Float64(), rand.Float64()}
res := 1; //result
forest.AddDataRow(data, res, 1000, 10, 2000) 
// AddDataRow : add new row, trim oldest row if there is more than 1000 rows, calculate a new 10 trees, but remove oldest trees if there is more than 2000 trees.
```


## Saving/Loading

### Saving 
Will Save the forest structure into binary file

File name format:
```
forest-UUID[:8]+sha256.bin
```

```go
	if fileName, err := forest.Save("saved/"); err != nil {
		t.Error(err)
		return
	}
```

### Loading
Will load forest structure from binary file :

```go
	if forest, errForest = Load("saved/forestTest.bin"); errForest != nil {
		return
	}
	
	fmt.Println("Vote", forest.Vote([]float64{0.9, 0.9, 0.9, 0.9}))
```
