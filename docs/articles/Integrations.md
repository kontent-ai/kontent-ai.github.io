# Integrations at Kontent.ai

To get a comprehensive overview of all Kontent's integration points, please refer to the [documentation](https://kontent.ai/learn/tutorials/develop-apps/integrate/integrations-overview/). 

Please note, that all integrations provided by the Kontent.ai team on [github](https://github.com/kontent-ai) are essentially an *integration examples*, are open-sourced (under MIT licence), and are meant to be reviewed and adapted by the end-customer. 

List of all available integration examples can be found through the **kontent-ai-integration** github topic here: 

<p align="center">
<a href="https://github.com/topics/kontent-ai-integrations" target="_blank"><image src="https://img.shields.io/static/v1?label=&message=example integrations&color=3dcca8&style=for-the-badge" alt="Kontent.ai example integrations" width="200" aligh="center"/></a>
</p>

While working with the linked repositories, we kindly ask everybody to follow the following rules:

1) **Any URLs provided in the repos are not be used in production.** You should follow the steps provided in the integration's repository to deploy it yourself for testing, or use in production.

2) If an integration is **missing deploy instructions**, or you're having trouble with setting it up, please **create an issue** in the integration's repository.

3) If you wish to **use any premade integration in a production project**, you should perform a **code review and fork/deploy the source code on your own** as integrations are subject to change without any notice. It is also always a good idea to inspect a code you are planning to use seriously, especially if it connects to a 3rd party services, and/or requires some kind of special authorization (api keys, etc.)

4) Some of the integrations may require further configuration such as custom API keys, or be subject to CORS limitation. In those cases you will need to fork the source repository and adjust the configuration in your copy repository according to instructions in the element's README file.

5) Some of the integrations may contain a form of a **server/backend part** as well (_using Netlify functions, Azure functions, or Amazon Lambda functions_). In that case, the setup process will require deploying and configuring these services as well for the element to work. This should be always mentioned and described in the repository documentation as well.

6) Feel free to share your created Kontent.ai integration with other developers. This can be easily done by marking your public github repository with **kontent-ai-integration** topic. 

7) When creating a new integration, feel free to use our [Template Repository for React](https://github.com/kontent-ai/integration-template-react) that will make it easier to set everything up. 

<br />

# Custom Elements
A Custom element is essentially a small HTML application that exists in a sandboxed `<iframe>` and interacts with [Kontent.ai](https://kontent.ai/) via the [Custom Elements API](https://kontent.ai/learn/reference/custom-elements-js-api). Their main purpose is to extend the content editing experience of your editors. 

Read [our dedicated tutorial](./New-Custom-Element.md) on how to create a custom element for Kontent.ai.

Also, have a look at the general overview of [deploying custom elements to Kontent.ai](https://kontent.ai/learn/tutorials/develop-apps/integrate/content-editing-extensions).

### Styling your custom elements

By including default styles from Kontent.ai, you can make your custom element look more like rest of the Kontent.ai Application UI.

The following [folder](https://github.com/kontent-ai/integration-template-react/tree/main/public) in our [**Integration Template repository**](https://github.com/kontent-ai/integration-template-react) contains all required files:

* [custom-element-v2.css](https://github.com/kontent-ai/integration-template-react/blob/main/public/kontent-ai-app-styles.css) – a CSS stylesheet
* [kentico-icons-v3.0.1.woff2](https://github.com/kontent-ai/integration-template-react/blob/main/public/kontent-ai-icons-v3.0.1.woff2) – a font file
* [examples.html](https://github.com/kontent-ai/integration-template-react/blob/main/public/examples.html) – An HTML page containing the implementation details and an HTML markup of some of the basic elements. See also the link in Demo section.