qEs5Io5yM8

Brute Force 0000~9999

```linux
for i in {0000..9999}; do
if ./leviathan6 "$i" | grep -q "Wrong"; then
echo "$i,Wrong"; 
continue; 
else 
echo "$i, Right"; 
break; 
fi; 
done
```


echo "$i" -> 따옴표랑 변수 호출 기호($)
