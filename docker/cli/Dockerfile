FROM golang:alpine AS build

RUN apk add \
        curl \
        git \
        build-base \
        upx
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/tomnomnom/meg@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/tomnomnom/assetfinder@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/tomnomnom/gf@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/tomnomnom/httprobe@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/tomnomnom/waybackurls@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/tomnomnom/unfurl@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/tomnomnom/anew@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/jaeles-project/gospider@latest
RUN git clone https://github.com/zmap/zdns.git /go/src/zdns && \
    cd /go/src/zdns && \
    CGO_ENABLED=1 GOOS=linux go build -ldflags="-s -w"
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/ffuf/ffuf@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/hakluke/hakrawler@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/003random/getJS@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/zricethezav/gitleaks@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/lc/gau@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/projectdiscovery/nuclei/v2/cmd/nuclei@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/projectdiscovery/shuffledns/cmd/shuffledns@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/projectdiscovery/naabu/cmd/naabu@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/projectdiscovery/chaos-client/cmd/chaos@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/projectdiscovery/dnsx/cmd/dnsx@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/projectdiscovery/httpx/cmd/httpx@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/projectdiscovery/tlsx/cmd/tlsx@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/projectdiscovery/katana/cmd/katana@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/projectdiscovery/asnmap/cmd/asnmap@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/projectdiscovery/proxify/cmd/proxify@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/projectdiscovery/cloudlist/cmd/cloudlist@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/projectdiscovery/uncover/cmd/uncover@latest
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/owasp-amass/amass/v3/...@master
RUN CGO_ENABLED=1 GOOS=linux go install -ldflags="-s -w" github.com/hahwul/dalfox/v2@latest
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y \
    && . $HOME/.cargo/env \
    && cd /tmp \
    && git clone https://github.com/RustScan/RustScan \
    && cd RustScan \
    && cargo build --release
RUN git clone https://github.com/blechschmidt/massdns /tmp/massdns \
    && cd /tmp/massdns \
    && make
RUN mkdir -p /root/bin
RUN cp /go/bin/* /root/bin/
RUN cp /tmp/RustScan/target/release/rustscan /root/bin/
RUN cp /tmp/massdns/bin/massdns /root/bin/
RUN upx /root/bin/*

FROM infosux/haxtools:base

COPY --from=build /go/pkg/mod/github.com/tomnomnom/meg@v0.3.0/lists/ /opt/meg/
COPY --from=build /root/bin/ /usr/bin/

RUN export DEBIAN_FRONTEND="noninteractive" \
    && apt-get -yqq update \
    && apt-get --no-install-recommends -yqq install \
        apktool \
        cewl \
        commix \
        crackmapexec \
        dirb \
        gdb \
        gdb-peda \
        gobuster \
        hashcat \
        hashcat-utils \
        hashid \
        hydra \
        john \
        joomscan \
        masscan \
        mitmproxy \
        pdfcrack \
        pngcheck \
        powershell-empire \
        pwncat \
        python3-impacket \
        python3-pwntools \
        radare2 \
        responder \
        smbmap \
        sqlmap \
        sslscan \
        stegcracker \
        steghide \
        stegsnow \
        wafw00f \
        wfuzz \
        whatweb \
        wpscan \
    && apt-get -y autoremove --purge \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /root/.cache \
    && rm -rf /var/cache/* \
    && rm -rf /var/log/* \
    && rm -rf /usr/share/doc-base/* \
    && rm -rf /usr/share/groff/* \
    && rm -rf /usr/share/info/* \
    && rm -rf /usr/share/lintian/* \
    && rm -rf /usr/share/linda/* \
    && rm -rf /var/cache/man/* \
    && find /usr/share/doc -depth -type f ! -name copyright -exec rm {} \; \
    && find /usr/share/locale -mindepth 1 -maxdepth 1 ! -name 'en_GB' ! -name 'en_US' -exec rm -r {} \; \
    && find /usr/share/i18n/locales -mindepth 1 -maxdepth 1 ! -name 'en_GB' ! -name 'en_US' -exec rm -r {} \; \
    && rm -rf /var/lib/gems/*/doc \
    && rm -rf /usr/share/icons/Adwaita/*

# RUN export DEBIAN_FRONTEND="noninteractive" \
#     && apt-get -yqq update \
#     && apt-get --no-install-recommends -yqq install \
#         build-essential \
#         python3-dev \
#     && pip3 install --upgrade --no-cache-dir aclpwn \
#     && pip3 install --upgrade --no-cache-dir aiodnsbrute \
#     && pip3 install --upgrade --no-cache-dir stego-lsb \
#     && apt-get -y remove \
#         build-essential \
#         python3-dev \
#     && apt-get -y autoremove --purge \
#     && apt-get clean \
#     && rm -rf /var/lib/apt/lists/* \
#     && rm -rf /root/.cache \
#     && rm -rf /var/cache/* \
#     && rm -rf /var/log/* \
#     && rm -rf /usr/share/doc-base/* \
#     && rm -rf /usr/share/groff/* \
#     && rm -rf /usr/share/info/* \
#     && rm -rf /usr/share/lintian/* \
#     && rm -rf /usr/share/linda/* \
#     && rm -rf /var/cache/man/* \
#     && find /usr/share/doc -depth -type f ! -name copyright -exec rm {} \; \
#     && find /usr/share/locale -mindepth 1 -maxdepth 1 ! -name 'en_GB' ! -name 'en_US' -exec rm -r {} \; \
#     && find /usr/share/i18n/locales -mindepth 1 -maxdepth 1 ! -name 'en_GB' ! -name 'en_US' -exec rm -r {} \; \
#     && rm -rf /var/lib/gems/*/doc

RUN git clone --depth 1 https://gitlab.com/exploit-database/exploitdb /usr/share/exploitdb \
    && ln -sf /usr/share/exploitdb/searchsploit /usr/bin/searchsploit \
    && rm -rf /usr/share/exploitdb/.git

RUN gem install --no-document evil-winrm

RUN gem install --no-document zsteg

RUN echo MaxCircuitDirtiness 10 >> /etc/tor/torrc \
    && update-rc.d tor enable \
    && sed -i 's/^strict_chain/#strict_chain/g;s/^round_robin_chain/round_robin_chain/g' /etc/proxychains4.conf

RUN update-ca-certificates \
    && rm -rf /usr/local/share/ca-certificates/*.crt

# ghidra
# RUN cd /opt && \
    # wget -O /opt/ghidra.zip https://github.com$(curl -s https://github.com/NationalSecurityAgency/ghidra/releases/ | grep "<a href.*zip" -m 1 | cut -d '"' -f 2) && \
    # unzip -qq ghidra.zip && \
    # rm -rf ghidra.zip && \
    # mv /opt/ghidra_* /opt/ghidra

# pyinstxtractor
# RUN wget "https://raw.githubusercontent.com/countercept/python-exe-unpacker/master/pyinstxtractor.py" -O /usr/bin/pyinstxtractor && \
#     chmod +x /usr/bin/pyinstxtractor

# RUN git clone --depth 1 https://github.com/danielmiessler/SecLists.git /opt/seclists && \
#     cd /opt/seclists/Passwords/Leaked-Databases && \
#     tar -xvzf rockyou.txt.tar.gz && \
#     rm rockyou.txt.tar.gz && \
#     rm rockyou-withcount.txt.tar.gz && \
#     rm -rf /opt/seclists/.git
    # dns nameservers
# RUN wget "https://public-dns.info/nameservers.csv" -O /tmp/nameservers.csv && \
#     grep ",1.00," /tmp/nameservers.csv | cut -d ',' -f 1 > /opt/nameservers-all.txt && \
#     grep -E -o "([0-9]{1,3}[\.]){3}[0-9]{1,3}" /opt/nameservers-all.txt > /opt/nameservers.txt && \
#     rm -rf /tmp/nameservers.csv

# RUN mkdir -p /opt/windows && \
#     wget "https://raw.githubusercontent.com/BloodHoundAD/BloodHound/master/Collectors/SharpHound.exe" -O /opt/windows/sharphound.exe && \
#     wget "https://raw.githubusercontent.com/BloodHoundAD/BloodHound/master/Collectors/SharpHound.ps1" -O /opt/windows/sharphound.ps1
    # jaws
# RUN git clone --depth 1 https://github.com/411Hall/JAWS.git /opt/windows/jaws && \
#     rm -rf /opt/windows/jaws/.git
    # nishang
# RUN git clone --depth 1 https://github.com/samratashok/nishang.git /opt/windows/nishang && \
#     rm -rf /opt/windows/nishang/.git
    # winpeas
# RUN wget "https://raw.githubusercontent.com/carlospolop/PEASS-ng/master/winPEAS/winPEASbat/winPEAS.bat" -O /opt/windows/winpeas.bat

# RUN mkdir -p /opt/linux && \
#     wget "https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh" -O /opt/linux/linenum.sh
    # lse
# RUN wget "https://raw.githubusercontent.com/diego-treitos/linux-smart-enumeration/master/lse.sh" -O /opt/linux/lse.sh
    # linpeas
# RUN wget "https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh" -O /opt/linux/linpeas.sh
    # pspy
# RUN wget "https://github.com/DominicBreuker/pspy/releases/download/v1.2.0/pspy32" -O /opt/linux/pspy32 && \
#     wget "https://github.com/DominicBreuker/pspy/releases/download/v1.2.0/pspy32s" -O /opt/linux/pspy32s && \
#     wget "https://github.com/DominicBreuker/pspy/releases/download/v1.2.0/pspy64" -O /opt/linux/pspy64 && \
#     wget "https://github.com/DominicBreuker/pspy/releases/download/v1.2.0/pspy64s" -O /opt/linux/pspy64s

# RUN wget "https://raw.githubusercontent.com/stealthcopter/deepce/main/deepce.sh" -O /opt/linux/deepce.sh
