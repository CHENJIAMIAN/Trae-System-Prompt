注入这个即可让第三方接到请求：
t.context.custom_model.base_url=xx
,t.data.custom_model.base_url=xx

```js
 async C(e, t) {
        const s = this.Q(t);
        let n = !0;
        const r = Date.now()
          , o = C => {
            const {code: S, reason: k} = C;
            if (S === 1e3)
                return;
            if (S === $y.BIG_MESSAGE_ERROR) {
                const O = tL.create($y.BIG_MESSAGE_ERROR, k);
                e.getController().error(O),
                this.W(C, s);
                return
            }
            const D = S === 1006
              , M = tL.create(D ? S : $y.SOCKET_CLIENT_CLOSE, k);
            e.getController().error(M),
            D || this.f.error("[ai-agent]  websocket rpc client onclose: ", S, k)
        }
          , a = C => {
            const S = tL.createWithError($y.SOCKET_CLIENT_ERROR, C);
            e.getController().error(S),
            this.W(C, s)
        }
          , l = C => {
            const S = tL.createWithError($y.SOCKET_ERROR, C);
            e.getController().error(S),
            this.Z(C, s)
        }
          , c = C => {
            C?.code !== 1006 && this.f.error("[ai-chat] request error: ", C?.code, C)
        }
        ;
        this.f.info(`[ai-chat] doRequestWithStream start: service=${t.service}, method=${t.method}, cost=${Date.now() - r}`);
        const [h,d,f] = await Promise.all([this.D(), this.L(), this.z.resolveDeviceId()]);
        this.f.info(`[ai-chat] doRequestWithStream getRPCClient: service=${t.service}, method=${t.method}, cost=${Date.now() - r}`);
        const g = {
            module_port: W1i,
            name: "aiAgent",
            getUrl: async () => (await h.client.ws.resolveAddress()).replace(/manager/, "module/ai-agent"),
            WebSocketCtor: WebSocket
        }
          , p = localStorage["enable-icube-manager-log"] ? this.f : void 0
          , {getWS: m, closeWS: b, sendJSON: v, startMessageLoop: w} = V1i(h, g, p);
        new Promise(async (C, S) => {
            const k = O => {
                l(O),
                S(O)
            }
            ;
            h.client.once("close", O => {
                S(O),
                o(O)
            }
            ),
            h.client.once("error", O => {
                S(O),
                a(O)
            }
            ),
            w(async (O, K) => {
                const T = O;
                if (!T)
                    return;
                if (T.code !== 0) {
                    const H = tL.create(T.code, T.message, void 0, void 0, T.data);
                    e.getController().error(H),
                    S(H),
                    this.Y(T, s);
                    return
                }
                n && (n = !1,
                this.ab(s)),
                T.data && e.getController().write(T.data);
                const P = T.data;
                P?.event === vlt.Done && (e.getController().complete(T.data || void 0),
                C(P.payload),
                this.X(T, s))
            }
            ).catch(k);
            const M = await m().catch(k);
            if (this.f.info(`[ai-chat] doRequestWithStream getWS: service=${t.service}, method=${t.method}, cost=${Date.now() - r}`),
            M) {
                M.onerror = k,
                M.onclose = o;
                const O = await this.I(t, d, f);
                this.f.info(`[ai-chat] doRequestWithStream sendJSON: service=${t.service}, method=${t.method}, cost=${Date.now() - r}`),
                v(O)
            }
        }
        ).catch(C => c(C)).finally( () => {
            h.client.off("close", o),
            h.client.off("error", a),
            b?.()
        }
        )
    }
```
**@Agent：定义未来智能体的规则与工具**

**一、 核心理念与架构**

*   **智能体 (Agent) 的构成公式：** Agent = PE (Prompt Engineering) + Tools
    *   **Prompt Engineering (PE):** 提供智能体清晰的上下文、目标和约束，使其能够理解特定的工作领域。
    *   **Tools:** 为智能体提供执行专业任务所需的必要能力和资源。

**二、 定制化智能体的构建过程**

*   **开发者赋能：** Trae 允许开发者超越内置智能体，创建定制化智能体以适应独特的开发流程。
*   **构建步骤：**
    *   **定义规则 (Rules):** 塑造智能体行为，涵盖编码风格、复杂性限制和架构指导等。
        *   **重要性：** 规则是智能体行为的基础，而不仅仅是 Prompt Engineering 的一部分，为灵活系统创造了条件。
    *   **选择工具 (Tools):** 确定智能体可访问的资源，包括不同的模型、API 和能力。

**三、 MCP (Model Context Protocol) 集成**

*   **作用：** 作为连接智能体的“神经网络”，建立通用的通信框架，实现智能体与第三方扩展的无缝交互。
*   **功能：** 智能协调最佳的智能体组合，根据用户需求将它们编排成统一的问题解决团队。

**四、 Agent-First 架构**

*   **核心能力：** @Agent 是 Trae 的核心。
*   **Trae 的独特之处：**
    *   **MCP 的角色：** MCP 作为底层的协调层，而非用户界面。
    *   **架构优势：** 这种设计使复杂的 AI 协作更直观且结果导向。
*   **智能体封装：** 将 MCP、本地工具和上下文信息打包在每个智能体内部。
    *   **结果：** 创造出能自主处理特定场景下最佳规则和工具组合的专业 AI 协作伙伴。
    *   **模块化设计：** 每个智能体专注于其特定领域，避免不必要的复杂性。
*   **用户体验：**
    *   **直接交互：** 用户与专门构建的智能体互动，而非直接管理底层协议。
    *   **高效协同：** 实现多智能体协同的强大功能，同时减轻了配置负担，让用户专注于创造。

**五、 开放智能体生态系统的构建**

*   **目标：** 建立一个充满活力的生态系统，通过协作涌现无限智能。
*   **社区贡献：** 鼓励开发者创建定制智能体，指数级扩展 Trae 的能力。
*   **愿景：** 建立一个 AI 编程生态系统，用户可以自由创建、共享和使用各种智能体，为所有人提供可访问的能力网络。
*   **最终目标：** 构建一个更高效、开放、智能的编程未来，模糊想象与创造之间的界限。

**六、 资源与支持 (末尾信息)**

*   博客、Trae IDE 下载、服务条款（服务条款、隐私政策、Cookie 政策）、支持（反馈）、文档、社区互动（Discord、Twitter X）。
