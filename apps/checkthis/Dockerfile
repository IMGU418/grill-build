# grill이 만든 커스텀 노드 이미지: 공용 grill ComfyUI 이미지 + 이 워크플로가 쓰는 커스텀 노드
# 빌드 폴더: 이 Dockerfile + workflow.json
FROM ghcr.io/imgu418/grill-comfy@sha256:d2c0ee9b4a3782a810a3b92d18be3ece2b38a7b89bf77f6618d3b3218b863452
COPY workflow.json /tmp/grill-build/workflow.json
RUN pip install comfyui_manager==4.2.2 \
  && export COMFYUI_PATH=/opt/ComfyUI \
  && python -m cm_cli deps-in-workflow --workflow /tmp/grill-build/workflow.json --output /tmp/grill-build/deps.json --mode remote \
  && python -m cm_cli install-deps /tmp/grill-build/deps.json --mode remote \
  && pip uninstall -y comfyui_manager \
  && for d in /opt/ComfyUI/custom_nodes/*/; do if [ -d "$d.git" ]; then echo "$(git -C "$d" remote get-url origin) $(git -C "$d" rev-parse HEAD)" >> /opt/ComfyUI/custom_nodes/grill-nodes.txt; fi; done \
  && rm -rf /root/.cache /opt/ComfyUI/user/__manager
RUN python -m compileall -q /opt/ComfyUI/custom_nodes > /dev/null || true
# 노드 코드에 적힌 모델 다운로드 주소와 Hugging Face 저장소 (grill이 모델 받을 곳을 찾는 데 쓴다): GRILL-URL / GRILL-REPO 줄
RUN echo aW1wb3J0IG9zLCByZQpFWFQgPSAoIi5zYWZldGVuc29ycyIsICIuc2Z0IiwgIi5wdCIsICIucHRoIiwgIi5ja3B0IiwgIi5iaW4iLCAiLm9ubngiLCAiLmdndWYiLCAiLnBrbCIpCnVybCA9IHJlLmNvbXBpbGUociJodHRwcz86Ly9bXlxzXCInPD4oKVxbXF17fVxcXSsiKQpyZXBvID0gcmUuY29tcGlsZShyInJlcG9faWRccyo9XHMqW1wiJ10oW1x3Li1dKy9bXHcuLV0rKVtcIiddIikKaGYgPSByZS5jb21waWxlKHIiaHR0cHM/Oi8vKD86aHVnZ2luZ2ZhY2VcLmNvfGhmXC5jbykvKD8hYXBpL3xzcGFjZXMvfGRhdGFzZXRzLykoW1x3Li1dKy9bXHcuLV0rKSIpCnVybHMsIHJlcG9zID0gc2V0KCksIHNldCgpCmZvciByb290LCBkaXJzLCBmaWxlcyBpbiBvcy53YWxrKCIvb3B0L0NvbWZ5VUkvY3VzdG9tX25vZGVzIik6CiAgICBkaXJzWzpdID0gW2QgZm9yIGQgaW4gZGlycyBpZiBub3QgZC5zdGFydHN3aXRoKCIuIikgYW5kIGQgIT0gIl9fcHljYWNoZV9fIl0KICAgIGZvciBmIGluIGZpbGVzOgogICAgICAgIGlmIG5vdCBmLmVuZHN3aXRoKCgiLnB5IiwgIi5qc29uIiwgIi55YW1sIiwgIi55bWwiKSk6CiAgICAgICAgICAgIGNvbnRpbnVlCiAgICAgICAgdHJ5OgogICAgICAgICAgICB0ID0gb3Blbihvcy5wYXRoLmpvaW4ocm9vdCwgZiksIGVuY29kaW5nPSJ1dGYtOCIsIGVycm9ycz0iaWdub3JlIikucmVhZCgyMDAwMDAwKQogICAgICAgIGV4Y2VwdCBPU0Vycm9yOgogICAgICAgICAgICBjb250aW51ZQogICAgICAgIGZvciB1IGluIHVybC5maW5kYWxsKHQpOgogICAgICAgICAgICB1ID0gdS5yc3RyaXAoIi4sOzonXCJgIikKICAgICAgICAgICAgaWYgdS5zcGxpdCgiPyIpWzBdLmxvd2VyKCkuZW5kc3dpdGgoRVhUKToKICAgICAgICAgICAgICAgIHVybHMuYWRkKHUpCiAgICAgICAgcmVwb3MudXBkYXRlKHJlcG8uZmluZGFsbCh0KSkKICAgICAgICByZXBvcy51cGRhdGUoaGYuZmluZGFsbCh0KSkKZm9yIHUgaW4gc29ydGVkKHVybHMpWzozMDAwXToKICAgIHByaW50KCJHUklMTC1VUkwiLCB1KQpmb3IgciBpbiBzb3J0ZWQocmVwb3MpWzoxMDAwXToKICAgIHByaW50KCJHUklMTC1SRVBPIiwgcikK | base64 -d | python
# 워크플로의 노드가 모두 들어갔는지 확인 (없으면 빌드 실패)
RUN python -m grill_comfy --check /tmp/grill-build/workflow.json && rm -rf /tmp/grill-build /tmp/grill-comfy
