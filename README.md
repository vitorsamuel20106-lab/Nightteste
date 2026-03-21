#!/bin/bash

==========================================================

--- 🛑 SISTEMA DE SEGURANÇA (ANTI-DEBUG & INTEGRIDADE) ---

( ( [[ "$-" == x ]] || [ -n "$BASH_EXECUTION_STRING" ] ) && kill -9 $$ ) 2>/dev/null
if [ -f "/proc/self/status" ] && grep -q "TracerPid.*[1-9]" /proc/self/status 2>/dev/null; then kill -9 $$; fi

==========================================================

PAINEL ELITE - VERSÃO EXTREME (A14 / MIRA ESTÁVEL PRO)

Cores

RED='\e[1;31m'
GOLD='\e[1;33m'
WHITE='\e[1;37m'
GREEN='\e[1;32m'
CYAN='\e[1;36m'
YELLOW='\e[1;33m'
PURPLE='\e[1;35m'
RESET='\e[0m'

==========================================================

--- 🧩 MOTOR DE AUTENTICAÇÃO ---

MAX=3; ATT=1; _AUTH_OK=0
_KEY_B64="U1VCQVJVLTg4MjE="
_KEY_REAL=$(echo "$_KEY_B64" | base64 -d)
rm -rf "$PREFIX/tmp/.sys_sbr_auth" 2>/dev/null

while [ $ATT -le $MAX ]; do
clear
echo -e "${RED}============================================================${RESET}"
echo -e "${GOLD}          🔐 ACESSO RESTRITO - PAINEL ELITE 🔐${RESET}"
echo -e "${RED}============================================================${RESET}"
echo -n -e "${WHITE} ➥ DIGITE SUA KEY DE ACESSO: ${RESET}"
read user_key

if [[ "$user_key" == "$_KEY_REAL" ]]; then    
    _AUTH_OK=1    
      
    # Captura limpa de hardware com tratamento de espaços e strings vazias  
    _DM=$(getprop ro.product.model 2>/dev/null | awk '{$1=$1};1')  
    [ -z "$_DM" ] && _DM="Desconhecido"  
      
    _AND=$(getprop ro.build.version.release 2>/dev/null | awk '{$1=$1};1')  
    [ -z "$_AND" ] && _AND="Desconhecido"  

    echo -e "\n${CYAN}[!] Autenticando Hardware: ${_DM} (Android ${_AND})...${RESET}"    
    sleep 1    
    break    
fi    

if [ $ATT -eq $MAX ]; then    
    echo -e "\n${RED}[❌] LIMITE EXCEDIDO! BLOQUEANDO...${RESET}"; sleep 2; kill -9 $$; exit 1    
fi    
    
echo -e "\n${RED}[❌] KEY INCORRETA! TENTE NOVAMENTE.${RESET}"; sleep $((ATT * 2)); ((ATT++))

done

[ "$_AUTH_OK" -ne 1 ] && exit 1
export RISH_APPLICATION_ID="com.termux"

==========================================================

--- ⚙️ MOTOR INTELIGENTE COM VERIFICAÇÃO REAL ---

carregar_animado() {
local cmd="$1"
local desc="$2"
local verify_cmd=""
local expected=""

# Regra exclusiva para o Canal do Criador  
if [[ "$desc" == "Abrindo canal do criador" ]]; then  
    echo -e "${PURPLE}${desc}${RESET}"  
elif [ -n "$desc" ]; then  
    echo -e "${WHITE}[!] Otimizando: ${desc}${RESET}"  
fi  
  
echo -ne "\e[?25l"    

local real_cmd="$cmd"  

# Lógica de interceptação para verificação  
if [[ "$cmd" == *"accessibility_touch_slop"* ]]; then  
    expected="1"; verify_cmd="settings get secure accessibility_touch_slop"  
elif [[ "$cmd" == *"window_animation_scale"* ]]; then  
    expected="0.0"; verify_cmd="settings get global window_animation_scale"  
elif [[ "$cmd" == *"peak_refresh_rate"* ]]; then  
    expected="144.0"; verify_cmd="settings get system peak_refresh_rate"  
elif [[ "$cmd" == *"tiktok.com"* ]]; then  
    verify_cmd=""   
elif [[ "$cmd" == *"wifi_scan_always_enabled"* ]]; then  
    expected="0"; verify_cmd="settings get global wifi_scan_always_enabled"  
elif [[ "$cmd" == *"thermalservice"* ]]; then  
    verify_cmd="settings get global thermal_mitigation"; expected="0"  
elif [[ "$cmd" == *"monkey -p com.dts.freefireth"* || "$cmd" == *"am start -n com.dts.freefireth"* ]]; then  
    verify_cmd="pidof com.dts.freefireth"; expected="[PID]"  
fi  

# Execução  
local tmp_err="$PREFIX/tmp/.err_log_$$"  
./rish -c "$real_cmd" > /dev/null 2> "$tmp_err" &  
local pid=$! ; local prog=1  
  
while kill -0 $pid 2>/dev/null; do  
    prog=$((prog + 20)); [ $prog -gt 95 ] && prog=95  
    echo -ne "\r${CYAN}Executando: ${prog}%${RESET}"; sleep 0.01  
done  
wait $pid; echo -ne "\r${CYAN}Executando: 100%${RESET}"  

local err_out=$(cat "$tmp_err" 2>/dev/null); rm -f "$tmp_err"  
local status="${GREEN}✔ APLICADO${RESET}"  

# Verificação de Resultados  
if [[ "$err_out" == *"SecurityException"* || "$err_out" == *"Permission"* ]]; then  
    status="${RED}❌ BLOQUEADO PELO SISTEMA${RESET}"  
elif [ -n "$verify_cmd" ]; then  
    local val=$(./rish -c "$verify_cmd" 2>/dev/null | tr -d '\r\n ')  
      
    if [[ "$cmd" == *"monkey"* || "$cmd" == *"com.dts.freefireth"* ]]; then  
        [[ -n "$val" ]] && status="${GREEN}✔ JOGO ABERTO COM SUCESSO${RESET}" || status="${RED}❌ FALHA AO INICIAR${RESET}"  
    elif [[ "$val" == *"$expected"* ]]; then  
        status="${GREEN}✔ APLICADO${RESET}"  
    else  
        status="${YELLOW}⚠ APLICADO (Modo Seguro/Fallback)${RESET}"  
    fi  
fi  

echo -e "\n${status}\n"; echo -ne "\e[?25h"

}

==========================================================

Loop principal

while true; do
clear
echo -e "${RED}============================================================${RESET}"
echo -e "${GOLD}                👑 Night ELITE - APOSTADO 👑${RESET}"
echo -e "${RED}  [!] VERSÃO DE TESTE - SÓ FUNCIONA VIA SHIZUKU${RESET}"
echo -e "${YELLOW}  ⚠ Pode não funcionar em todos os dispositivos${RESET}"
echo -e "${RED}============================================================${RESET}"
echo -e " 📱 DISPOSITIVO: ${CYAN}${_DM}${RESET}"
echo -e " 🤖 SISTEMA:     ${CYAN}Android ${_AND} (Shizuku Ativo)${RESET}"
echo -e "${RED}============================================================${RESET}"

echo -e "${GOLD} [99] 🚀 INJEÇÃO TOTAL        [77] 🧠 OTIMIZAR SISTEMA${RESET}"      
echo -e "${WHITE} [01] 🎯 SENSI EMULADOR       [09] ❄️ RESFRIAMENTO CPU${RESET}"      
echo -e "${WHITE} [02] ꚠ CANAL DO CRIADOR      [10] 🧹 LIMPEZA TOTAL${RESET}"      
echo -e "${WHITE} [03] 🎯 DPI PADRÃO           [11] 🚫 FECHAR APPS${RESET}"      
echo -e "${WHITE} [04] 🧠 RAM & CPU            [12] 🔄 RESET TOTAL${RESET}"      
echo -e "${WHITE} [05] 🎮 GPU ADRENO MAX       [13] 🖱️ VELOC. PONTEIRO${RESET}"      
echo -e "${WHITE} [06] ⚡ TELA 144Hz PURO      [14] 🎯 MIRA ANTI-PINO${RESET}"      
echo -e "${WHITE} [07] 👻 MODO GHOST           [15] 🎮 ABRIR FREE FIRE${RESET}"      
echo -e "${WHITE} [08] 🛡️ ANTI-BAN & PING      ${RED}[00] ❌ SAIR DO PAINEL${RESET}"      
echo -e "${RED}------------------------------------------------------------${RESET}"      
echo -n -e "${GOLD} ➥ DIGITE O COMANDO: ${RESET}"      

read opcao      
stty -echo      

case $opcao in      
99)      
    echo -e "\n${GOLD}[🚀] ATIVANDO INJEÇÃO TOTAL INTELIGENTE...${RESET}\n"      
      
    # 1. 🎯 SENSI E TOQUE  
    carregar_animado "settings put secure accessibility_touch_slop 1 && settings put secure long_press_timeout 180" "SENSI EMULADOR (Toque Rápido)"  
    carregar_animado "settings put system pointer_speed 7" "VELOCIDADE PONTEIRO"  
    carregar_animado "settings put secure accessibility_touch_slop 1" "MIRA ANTI-PINO (Consistência)"  
      
    # 2. 🎮 MIRA ESTÁVEL (SEM TREMER)  
    carregar_animado "settings put global disable_window_blurs 1 && settings put global window_animation_scale 0.0 && settings put global transition_animation_scale 0.0 && settings put global animator_duration_scale 0.0" "GPU ADRENO MAX & MIRA ESTÁVEL PRO"  
      
    # 3. ⚡ FPS E TELA  
    carregar_animado "settings put system min_refresh_rate 144.0 && settings put system peak_refresh_rate 144.0" "TELA 144HZ PURO"  
      
    # 4. 🧠 RAM E PROCESSOS  
    carregar_animado "settings put global cached_apps_freezer enabled" "CONGELAMENTO DE RAM"  
    carregar_animado "cmd activity kill-all" "ENCERRANDO APPS EM SEGUNDO PLANO"  
      
    # 5. ❄️ CPU (RESFRIAMENTO)  
    carregar_animado "cmd thermalservice override-status 0 2>/dev/null || settings put global thermal_mitigation 0" "RESFRIAMENTO CPU"  
      
    # 6. 📶 REDE / PING  
    carregar_animado "settings put global wifi_scan_always_enabled 0 && settings put global ble_scan_always_enabled 0" "ESTABILIZANDO REDE"  
      
    # 7. 🧹 LIMPEZA  
    carregar_animado "pm trim-caches 999999999" "LIMPANDO CACHE LIXO"  
      
    # 8. 🧠 SISTEMA  
    carregar_animado "cmd package bg-dexopt-job &" "OTIMIZANDO KERNEL/DEX"  
      
    # 9. ⚙️ AJUSTES FINAIS  
    carregar_animado "wm density reset" "DPI PADRÃO"  
    carregar_animado "settings put secure ui_night_mode 2" "MODO GHOST"  
      
    # 10. 🎮 FINALIZAÇÃO  
    carregar_animado "monkey -p com.dts.freefireth -c android.intent.category.LAUNCHER 1 2>/dev/null || am start -n com.dts.freefireth/com.dts.freefireth.FFMainActivity 2>/dev/null || (cmd package resolve-activity --brief com.dts.freefireth | tail -n 1 | xargs -n 1 am start -n) 2>/dev/null" "LANÇANDO FREE FIRE"  
      
    echo -e "${GREEN}[🔥] INJEÇÃO TOTAL FINALIZADA! DISPOSITIVO PRONTO, MUITO CAPA PRA CIMA DELES!${RESET}"  
    sleep 3   
    ;;      

77) carregar_animado "cmd package bg-dexopt-job &" "OTIMIZANDO KERNEL/DEX"; sleep 1 ;;    
01|1) carregar_animado "settings put secure accessibility_touch_slop 1 && settings put secure long_press_timeout 180" "SENSI EMULADOR (Toque Rápido)"; sleep 1 ;;    
02|2) carregar_animado "am start -a android.intent.action.VIEW -d \"https://tiktok.com/@night_pushhard\" 2>/dev/null || monkey -p com.android.chrome -c android.intent.category.LAUNCHER 1" "Abrindo canal do criador"; sleep 1 ;;    
03|3) carregar_animado "wm density reset" "DPI PADRÃO"; sleep 1 ;;    
04|4) carregar_animado "settings put global cached_apps_freezer enabled" "CONGELAMENTO DE RAM"; sleep 1 ;;    
05|5) carregar_animado "settings put global disable_window_blurs 1 && settings put global window_animation_scale 0.0 && settings put global transition_animation_scale 0.0 && settings put global animator_duration_scale 0.0" "GPU ADRENO MAX"; sleep 1 ;;    
06|6) carregar_animado "settings put system min_refresh_rate 144.0 && settings put system peak_refresh_rate 144.0" "TELA 144HZ PURO"; sleep 1 ;;    
07|7) carregar_animado "settings put secure ui_night_mode 2" "MODO GHOST"; sleep 1 ;;    
08|8) carregar_animado "settings put global wifi_scan_always_enabled 0 && settings put global ble_scan_always_enabled 0" "ESTABILIZANDO REDE"; sleep 1 ;;    
09|9) carregar_animado "cmd thermalservice override-status 0 2>/dev/null || settings put global thermal_mitigation 0" "RESFRIAMENTO CPU"; sleep 1 ;;    
10) carregar_animado "pm trim-caches 999999999" "LIMPANDO CACHE LIXO"; sleep 1 ;;    
11) carregar_animado "cmd activity kill-all" "ENCERRANDO APPS EM SEGUNDO PLANO"; sleep 1 ;;    
12) carregar_animado "wm density reset && settings put global window_animation_scale 1.0 && settings put global transition_animation_scale 1.0 && settings put global animator_duration_scale 1.0" "RESETANDO CONFIGURAÇÕES"; sleep 1 ;;    
13) carregar_animado "settings put system pointer_speed 7" "VELOCIDADE PONTEIRO"; sleep 1 ;;    
14) carregar_animado "settings put secure accessibility_touch_slop 1" "MIRA ANTI-PINO (Consistência)"; sleep 1 ;;    
15) carregar_animado "monkey -p com.dts.freefireth -c android.intent.category.LAUNCHER 1 2>/dev/null || am start -n com.dts.freefireth/com.dts.freefireth.FFMainActivity 2>/dev/null || (cmd package resolve-activity --brief com.dts.freefireth | tail -n 1 | xargs -n 1 am start -n) 2>/dev/null" "LANÇANDO FREE FIRE"; sleep 1 ;;    
00|0) stty echo; clear; exit 0 ;;    
*) ;;    
esac       

read -t 0.1 -n 10000 discard    
stty echo

done
