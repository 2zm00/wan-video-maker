# Wan 2.2 영상 생성 프로젝트 (+ ComfyUI)


## 1. 환경 준비
- macOS (M4 칩, 24GB 메모리) 사용
- Python 3.12.11, PyTorch 2.8.0 (MPS 백엔드)
- 가상환경(UV) 생성 및 활성화

## 2. 필수 소프트웨어 설치
- 가상환경(UV) 내 ComfyUI 설치
- Wan 2.2 모델 레포 클론 및 필요한 모델 파일 다운로드
- ComfyUI 실행 준비 (포트: 기본 8188)

## 3. ComfyUI 설정 및 실행
- ComfyUI 실행:
```text
python main.py
```
- 웹브라우저에서 http://127.0.0.1:8188 접속
- 템플릿 메뉴 → Wan 2.2 워크플로 템플릿 불러오기
- Wan2.2 모델 파일 및 관련 파일을 ComfyUI 디렉토리 내 적절한 폴더에 배치
- LoRA 모델 다운로드 후 models/loras 폴더에 복사

## 4. 모델, LoRA, VAE 등 로드
- Wan 2.2 UNet 모델과 VAE, 텍스트 인코더 별도 로드
- LoRA는 Load LoRA 노드로 연결 후 Wan 2.2 모델과 연동
- LoRA와 기본 모델 버전(2.2)이 호환되는지 꼭 확인


## 5. VRAM 및 메모리 최적화 권고
- Mac M4의 MPS 환경 특성상 VRAM 부족 발생 가능성 있음
- Batch size 조절, 해상도 축소(예: 512x512 → 256x256) 권장
- 영상 길이 및 프레임 수 줄여서 메모리 부담 완화
- 필요 시 환경변수로 MPS 메모리 제한 해제 가능 (주의 필요)

```text
export PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.0
```

- ComfyUI 및 PyTorch 최신 버전 유지

## 6. 작업 흐름
- 워크플로우 설정 후 텍스트 프롬프트 작성
- 콘텐츠에 적합한 positive/negative prompt 준비
- 모델과 LoRA 연결 상태 확인 및 테스트 실행
- 실행 대기열에 작업 올리고 생성 결과 기다림
- 결과 퀄리티가 만족스럽지 않으면 프롬프트, LoRA 강도, 워크플로우 조절 반복

---

```
Total VRAM 24576 MB, total RAM 24576 MB
pytorch version: 2.8.0
Mac Version (15, 6, 1)
Set vram state to: SHARED
Device: mps
Using sub quadratic optimization for attention, if you have memory or speed issues try using: --use-split-cross-attention
Python version: 3.12.11 (main, Aug 18 2025, 19:02:39) [Clang 20.1.4 ]
ComfyUI version: 0.3.54
ComfyUI frontend version: 1.25.11
[Prompt Server] web root: /Users/zmo/Workspaces/wan-video-maker/.venv/lib/python3.12/site-packages/comfyui_frontend_package/static

Import times for custom nodes:
   0.0 seconds: /Users/zmo/Workspaces/wan-video-maker/ComfyUI/custom_nodes/websocket_image_save.py

Context impl SQLiteImpl.
Will assume non-transactional DDL.
No target revision found.
Starting server

To see the GUI go to: http://127.0.0.1:8188
got prompt
Using split attention in VAE
Using split attention in VAE
VAE load device: mps, offload device: cpu, dtype: torch.bfloat16
Using scaled fp8: fp8 matrix mult: False, scale input: False
Requested to load WanTEModel
loaded completely 9.5367431640625e+25 6419.477203369141 True
CLIP/text encoder model load device: cpu, offload device: cpu, current: cpu, dtype: torch.float16
model weight dtype torch.float16, manual cast: None
model_type FLOW
Requested to load WAN22
0 models unloaded.
loaded completely 9.5367431640625e+25 9536.402709960938 True
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████| 20/20 [46:35<00:00, 139.79s/it]
Requested to load WanVAE
loaded completely 9.5367431640625e+25 1344.0869674682617 True
```