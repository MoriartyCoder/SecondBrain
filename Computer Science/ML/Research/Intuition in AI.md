
# Approaches
The idea is to 

### Pre-run
A smaller (by 1/3?) neural network is getting the input first and is additionally to the planned output of the bigger model outputting a confidence score. If the score is under a threshold (0.85?) then the bigger net is used. 

#### Parameters
* downsize factor
* threshold

#### Variations
* reduced input: 
	Reduction of the input vector. Not enough input -> low confidence score. Maybe less data prep necessary because of the reduced input size.

## Integrated
Both networks run parallel until the smaller network finished. Then the process is interrupted to evaluate the confidence score. Based on  
## Stacked:
First the smaller network gets computed then the process is interrupted and the score is evaluated. If the score is below the threshold then the output of the smaller network is added to the input vector of the bigger network.