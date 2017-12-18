JP morgan
- chocolate bar row*col
- bottom left piece is poisioned
- you and your opponent pick any row or col to eat
- the last one eat the bottom left piece will die

write a program to see if you alive

```
bool live(int row, int col){
    if(row==1 && col==1) return false;    
    if(row==1 || col==1) return true;
    
    for(int r=1; r<=row-1; r++){
        //opponent
        if(!live(r,col)){
            return true;
        }
    }
        
    for(int c=1; c<=col-1; c++){
        //opponent
        if(!live(row,c)){
            return true;
        }   
    }
    
    return false;
}

int main()
{
  std::cout<<live(1,1)<<std::endl;
  std::cout<<live(1,3)<<std::endl;
  std::cout<<live(3,3)<<std::endl;
  std::cout<<live(3,4)<<std::endl;
}
```