#!/bin/bash


command -v shasum >/dev/null 2>&1 && shasum='shasum'
command -v sha1sum >/dev/null 2>&1 && sha1sum='sha1sum'
if [ ! -n $shasum ]; then
    if [ ! -n $sha1sum ]; then
        echo 'Nor shasum or sha1sum is installed. Aborting.'
        exit 1;
    fi
fi

command -v git >/dev/null 2>&1 || { echo >&2 "Git is required but it's not installed.  Aborting."; exit 1; }
command -v docker >/dev/null 2>&1 || { echo >&2 "Docker is required but it's not installed.  Aborting."; exit 1; }


rm -rf __tmp
mkdir __tmp
cd __tmp
echo 'start requesting image'
{
    git clone https://harmonycloudbot:th1s1sn0tag00dpassw0rd@github.com/harmonycloudbot/imagecamel.git
} &> /dev/null

cd imagecamel
if [ $shasum ]; then
    hash=$((0x$(shasum <<<$1)0))
  
else 
    touch tmpfile
    echo $1 >> tmpfile
    hash=$(sha1sum tmpfile | head -c 20)
fi
{
    git checkout -b b$hash
} &> /dev/null
 
rm -f Dockerfile

echo FROM $1 >> Dockerfile

{
    git add * 
    git commit -m 'build image' 
    git push origin b$hash:b$hash 
} &> /dev/null
echo 'image request scheduled'

echo waiting to pull image $1.
echo this may take 5-10 mins,due to the size of the image.
count=0
while [ 0 -eq 0 ]  
do  
    echo $count
    if [ $count -gt 50 ]; then  
        echo 'pull image fail.'
        exit 1 
    fi
    echo "start pulling image "$1  
    {
        docker pull harmonycloudbot/imagecamel:b$hash 
    }  &> /dev/null
    if [ $? -eq 0 ]; then  
        echo image  $1 pulled
        {
            docker tag harmonycloudbot/imagecamel:b$hash $1
            docker rmi harmonycloudbot/imagecamel:b$hash
        }  &> /dev/null
        break;  
    else  
        echo retry
        count=$(( $count + 1 ))
        sleep 30  
    fi  
done  
cd ../../
rm -rf __tmp
echo  $hash

