#MODULE 1
#최신 CPC table을 특허청에서 다운받아 불러오기
#import xls file
library(purrr)
library(readxl)
library(data.table)
file <- 'C:/Analysis/R_Analysis/221103_CPC_202208/Input/CPC분류표_섹션별_2022년8월.xlsx'
sheets <- excel_sheets(file)
Rawdata <- map_df(sheets, ~ read_excel(file, sheet = .x))






# #여러 개의 XLS파일을 CSV로 바꾸기
# setwd("C:/Analysis/R_Analysis/220706_CPC_202205/Input")
# library(readxl)
# files.to.read = list.files(pattern="xlsx")
# lapply(files.to.read, function(f) {
#  df = read_excel(f, sheet=1)
#  write.csv(df, gsub("xlsx", "csv", f), row.names=FALSE)
# })


# #여러개의 CSV파일을 하나의 파일로 Merging
# library(data.table)
# folder <- getwd()
# files <- list.files(path = folder, pattern="*.csv")
# Rawdata <-  rbindlist(lapply(files, fread), fill = TRUE)
# colnames(Rawdata)


# #경로
# setwd("C:/Analysis/R_Analysis/221103_CPC_202208/Input")
# 
# #Rawdata(특허 데이터) CSV로 저장하기
# write.csv(df.temp, file ="./Rawdata_total_CSV.csv",row.names=FALSE, fileEncoding="euc-kr")
# 
# #Rawdata(특허 데이터) CSV로 읽어들이기
# Rawdata = read.csv("./Rawdata_total_CSV.csv", row.names = NULL, quote = "", stringsAsFactors=FALSE, fileEncoding="euc-kr")








#경로
setwd("C:/Analysis/R_Analysis/221103_CPC_202208/")

#Subsetting
myvars <- c("코드", "도트", "원문", "번역")
Rawdata.temp <- Rawdata[, myvars]
Rawdata.temp <- Rawdata.temp[ which(is.na(Rawdata.temp$코드) == FALSE), ]
Rawdata.temp <- Rawdata.temp[ which(is.na(Rawdata.temp$도트) == FALSE ), ]
colnames(Rawdata.temp) <- c("cpc", "depth", "english", "korean")

library("writexl")
write_xlsx(Rawdata.temp, "./Output/CPC_tree_description_202208_XLS.xlsx")







#MODULE 2
#rawdata for Realgroup time-series analysis
#cpc / realgroup
library(data.table)
##### 데이터 불러오기, 전처리
data <- Rawdata.temp
colnames(data)
colnames(data) <- c("cpc", "depth", "this_description_english", "this_description_hangul")

data$nchar = nchar(data$cpc) #cpc 글자 수 (Section, Class, Subclass 등 상위 그룹 제거를 위함 - RealGroup에서는 가장 상위 그룹이 Maingroup임)
#data$description_hangul[which(data$description_hangul=="")] = data$description[which(data$description_hangul=="")] #한글 description이 없는 경우 영문으로 대체

section_tbl = data[which(data$nchar==1),]
class_tbl = data[which(data$nchar==3),]
subclass_tbl = data[which(data$nchar==4),]
#data = data[-which(data$nchar<=4),] #Maingroup 이하 CPC만 필터링

##### Realgroup 부여
data$id = ""
maingroup_idx_range = which(data$depth==0)
data$id[maingroup_idx_range] = seq_along(maingroup_idx_range) #Maingroup인 CPC에게만 1번부터 차례로 id 부여 (최상위 depth)
#nrow(data)
for(i in 1:nrow(data)){ #i=2
  this_depth = as.numeric(data$depth[i])
  if(this_depth==0){
    id_tree = as.numeric(c(data$id[i]))
  }else{
    if( (length(id_tree)-1) < this_depth ){
      id_tree = c(id_tree, 1)
    }else if( (length(id_tree)-1) == this_depth ){
      id_tree[length(id_tree)] = id_tree[length(id_tree)] + 1
    }else{
      id_tree = id_tree[1:(this_depth + 1)]
      id_tree[length(id_tree)] = id_tree[length(id_tree)] + 1
    }
    data$id[i] = paste0(id_tree, collapse="_")
  }
  cat("[", i, "/", nrow(data), "]\n", sep="")
}
#write.csv(data, "./Output/CPC_tree_description_202208(realgroup_중간과정)_CSV.csv", row.names=F, fileEncoding="euc-kr")

#하위 CPC를 갖는 CPC의 reagroup id에 "_" 붙이기
#data = read.csv("./Output/CPC_tree_description_202208(realgroup_중간과정)_CSV.csv", stringsAsFactors=F, fileEncoding="euc-kr")
data$realgroup <- data$id
data$realgroup.temp <- data$realgroup

for(n in 1:nrow(data))
{
  #n <-2
  m <- n+1
  if(data$depth[n] < data$depth[m])
  {
    data$realgroup.temp[n] <- paste0(data$realgroup[n], "_")
  }
  else
  {
    data$realgroup.temp[n] <- data$realgroup[n]
  }
  print(n)
}
#write.csv(data, "./Output/CPC_tree_description_202208(allcolumns_realgroup).csv", row.names=F, fileEncoding="euc-kr")
#data = read.csv("./Output/CPC_tree_description_2019(realgroup_중간과정2)_CSV.csv", stringsAsFactors=F)
library("writexl")
write_xlsx(data, "./Output/CPC_tree_description_202208(allcolumns_realgroup)_XLS.xlsx")

#write
data.realgroup <- data[c("cpc", "realgroup.temp")]
colnames(data.realgroup) <- c("cpc", "realgroup")
write.csv(data.realgroup, "./Output/CPC_tree_description_202208(realgroup)_CSV.csv", row.names=F, fileEncoding="euc-kr")









#MODULE 3
#rawdata for Subclass time-series analysis
#cpc / subclass
data.subclass <- Rawdata.temp
data.subclass$Subclass <- substr(data.subclass$cpc, 1, 4)
vars <- c("cpc", "Subclass")
#data.subclass <- data.subclass[,.vars]
data.subclass <- subset(data.subclass, select = vars)
colnames(data.subclass) <- c("cpc", "Subclass")
write.csv(data.subclass, file ="./Output/CPC_tree_description_202208(subclass)_CSV.csv",row.names=FALSE, fileEncoding="euc-kr")










#MODULE 4
#rawdata for Subgroup time-series analysis
#cpc / maingroup #마지막 maingroup 만드는 것이 오류가 나는데, 이는 엑셀에서 수정하였음.
data.subgroup <- Rawdata.temp
num <- which(data.subgroup$depth == "0" )
data.subgroup$maingroup <- data.subgroup$cpc

i <- 1
m <- 1
n <- num[i]
k <- num[i+1]
for(i in 1:nrow(data.subgroup))
{
  #i=80
  if(data.subgroup$depth[i] == "0"){
    data.subgroup$maingroup[i] <- data.subgroup$cpc[i]
    if(i == k-1){
      m <- m+1
      n <- num[m]
      k <- num[m+1]
    }
  }else{
    data.subgroup$maingroup[i] <- data.subgroup$cpc[n]
    if(i == k-1){
      m <- m+1
      n <- num[m]
      k <- num[m+1]
    }
  }
  print(i)
}
write.csv(data.subgroup, file ="./Output/CPC_tree_description_202208(subgroup)_CSV.csv",row.names=FALSE, fileEncoding="euc-kr")

