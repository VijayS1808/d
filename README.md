# GNU nano 6.3                                                            xss.sh                                                             Modified

NA='\033[0m'       # Text Reset


B='\033[1;30m'       # Black
R='\033[1;31m'         # Red
G='\033[1;32m'       # Green
Y='\033[1;33m'      # Yellow
B='\033[1;34m'        # Blue
P='\033[1;35m'      # Purple
C='\033[1;36m'        # Cyan
BWhite='\033[1;37m'       # White

A='\e[47m'  #white backgraound
Z='\e[46m' # Cyan

# echo -e "${RED}Red text${ENDCOLOR}"

echo -e "${R} Welcome to the hacking world !!! ${NA}"

echo -ne  "Enter the domain: $1 \n"

echo -e "${B} Subdomain Enumuration ${NA}"

echo -e "${R}"

subfinder -d $1 -silent | tee sub.txt

echo -e "${B} Sudomain from assetfinder ${NA}"

echo -e "${Y}"

assetfinder $1 | grep "$1" | tee -a sub.txt

echo -e "${B} Sudomain from Crt.sh ${NA}"

echo -e "${B}"

curl -s "https://crt.sh/?q=%25.HOST&output=json" | jq -r '.[].name_value' | sed 's/\*\.//g' | sort -u | grep  "$1" | tee -a sub.txt
cat  sub.txt | httpx --threads 300 --mc 200 | tee subdomains.txt


echo -e "${B} Crawl urls from Gau ${NA}"

rm sub.txt

echo -e "${P}"

cat subdomains.txt | gau --fp --threads 300 --blacklist png,jpg,gif,pdf,jpeg,woff,woff2,tiff,tiff2 | tee urls.txt

# echo -e "${B} Crawl urls from Waybackurls ${NA}"

#echo -e "${C}"

#cat subdomains.txt | waybackurls | tee -a urls.txt

echo -e "${B} Crawl urls from Katana ${NA}"

echo -e "${R}"

cat subdomains.txt | httpx | katana | tee -a urls.txt

echo -e "${R} Clean the urls ${NA}"

echo -e "${B}"

cat urls.txt | uniq | uro | tee -a clean.txt


rm urls.txt

cat clean.txt | grep "=" | tee -a kxss.txt

cat kxss.txt | httpx --mc 200 --threads 500 | tee  xss_urls.txt

rm kxss.txt clean.txt
rm subdomains.txt
