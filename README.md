# techsights.github.io

## 1.1. Website

---

Description for banker's algorithm

---

### 1.1.1. Reddit

---
Description of your function

The function below is used to open the file. It will display an error message with an exit status set to 1 if file is not found.

```C
    //Read in Data from text file
   FILE *input_fp;
   if ((input_fp = fopen("fertility_Diagnosis_Data_Group1_4.txt", "r")) == NULL){
       printf("\nError in opening the file.");
       exit(1);
   }
```

---