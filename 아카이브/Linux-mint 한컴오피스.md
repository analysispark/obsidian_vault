# Linux-mint 한컴오피스  
  
// hwp 다운로드  
curl -H "Host: cdn.hancom.com" -H "Referer: https://www.hancom.com/cs_center" -fLO [https://cdn.hancom.com/pds/hnc/DOWN/gooroom/hoffice_hwp_2020_amd64.deb](https://cdn.hancom.com/pds/hnc/DOWN/gooroom/hoffice_hwp_2020_amd64.deb)  
  
sudo dpkg -i hoffice_hwp_2020_amd64.deb  
  
  
// 영문버전을 한글 버전으로  
sudo vi /usr/share/applications/hoffice11-hwp.desktop  
  
**Exec=/bin/bash -c "LANGUAGE=ko_KR /opt/hnc/hoffice11/Bin/hwp %f"**  로 수정  
  
  
// 한영 입력  
  
sudo apt update  
sudo apt install qtbase5-dev qtwayland5  
  
cd /opt/hnc/hoffice11/Bin/  
sudo mv qt qt.bak  
