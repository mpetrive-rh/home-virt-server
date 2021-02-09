FROM quay.io/ansible/ansible-runner:devel as galaxy

ADD _build/requirements.yml /build/_build/requirements.yml

RUN ansible-galaxy role install -r /build/_build/requirements.yml --roles-path /usr/share/ansible/roles
RUN ansible-galaxy collection install -r /build/_build/requirements.yml --collections-path /usr/share/ansible/collections

RUN mkdir -p /usr/share/ansible/roles /usr/share/ansible/collections

FROM quay.io/ansible/python-builder:latest as builder

ADD _build/requirements_combined.txt /tmp/src/requirements.txt
RUN assemble

FROM quay.io/ansible/ansible-runner:devel

RUN pip3 install --upgrade pip setuptools

COPY --from=galaxy /usr/share/ansible/roles /usr/share/ansible/roles
COPY --from=galaxy /usr/share/ansible/collections /usr/share/ansible/collections

COPY --from=builder /output/ /output/
RUN /output/install-from-bindep && rm -rf /output/wheels
RUN ls -la /etc
