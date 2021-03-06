# <img src="images/Rollout-blue.svg" alt="CloudBees Rollout Logo" width="40" align="top"> Using CloudBees Rollout to Control Flags

## Using CloudBees Rollout to Control Feature Flag Values

In this lab, you will use the CloudBees Rollout dashboard to remotely configure the values of the `title` and `sidebar` feature flags. Additionally, we will introduce the Flag Override tool, and walk through a scenario where developers may need to alter a flag's for local testing without affecting the values for others.

### Creating a Rollout Experiment

1. Switch tabs to bring up the Rollout Dashboard.
2. On the left-hand side of the screen, click the **Development** environment, and then select **Experiments** from the expanded list. From the view that follows, click the **Create a New Experiment** button.
3. In the pop-up menu, choose the **default.title** flag from the drop-down. To set up the experiment, choose the **Set Audience** button.

<p><img src="images/createNewTitleExp.png" />

4. Right now, the new title is hidden for all. And the title experiment reflects this: the only condition uses the `title` flag's default value (False). This default experiment is set for the audience defined on **All Versions** (the microblog only has 1 version) and **All Users**.
5. Let's change the flag's experiment so that all users will see the new title. Click the current **False** behavior value, and from the drop-down menu, choose **True** to edit the `title` flag experiment.
6. When changes to an experiment are applied, a new configuration file is written and delivered to the devices. Select **Update Audience** button to send the new configuration with its updated `title` flag value.

<p><img src="images/setTitleTrue.png" />

7. Switch tabs to bring up the Microblog website. Thanks to the `configurationFetchedHandler` implemented in the previous lab, the page refreshes automatically and the new configuration is applied. The new title should appear!
8. Switch tabs to bring CloudBees Core into view. The _most recent run_ from the microblog's `development` branch should be the pipeline induced by the configurationFetchedHandler commit from the previous lab. We have successfully changed the behavior of feature flag gated code dashboard and **without additional code deployments**.
9. Navigate back to the Rollout dashboard. And navigate to **Experiments** view under **Development** environment, and choose a **Create a New Experiment** button. Ensure the new experiment will change the behavior for the **default.sidebar** flag before continuing to **Set Audience**.
10. Similar to the title experiment, edit the **sidebar** by changing the only condition's **False** behavior value to **True**. Apply the experiment's changes through the **Update Audience** button.
11. Switch back to the Microblog website, and after the auto-refresh, the sidebar should now be displayed!

### Flag Override View Interface

The experiments seen in a Rollout environment view govern the behavior of all code connected using that environment's `<ROLLOUT_ENV_KEY>`. However, a developer or tester may need to specify unique flag values running on his or her local machine, _without altering the values for others_. Rollout allows developers and testers to leverage a **Flag Ovverride** view to act as _local_ Rollout dashboard and easily change flag values. But, it's important to _restrict_ this **Flag Override** tool, as it would probably want to be hidden from end-users.

1. Switch tabs so the Microblog website is in view. Notice the **DEV** panel in header. Click it, and the **Flag Override** window should appear in the lower right-hand corner of the screen.
2. Right now, both `title` and `sidebar` flags are configured to **True** from their respective development experiments configured through Rollout. Change the `title` flag's _local_ value to **False**, and **refresh** the page.
3. To simulate another tester on a different device, open the Microblog website in a _different_ internet browser or _private browsing session_ (Don't close the existing session). In the new browsing session, the `title` and `sidebar` are both **True** due to the configuration applied from the experiments in the Rollout dashboard.
4. In new session, again bring up **Flags Override** view (selecting DEV panel in the header). Change the local flag values so that they are inverted from previous browsing session (`title` is **True**, set `sidebar` is **False**). Refresh the page.
5. Each user (represented by the microblog in separate browsers) has set their own personal feature flag values for local testing or development. Close the newer browser when finished. In original session, open **FlagOverride**. Then select _Reset All Overrides_, close the **FlagOverride** view, and finally refresh the page.

### Learning Flags Override Code Implementation

* Bring the microblog-frontend repository and its root directory view in Github.
* On the `development` branch, navigate to `src/components/Nav.vue` by clicking the `src` folder, then `components` folder, and finally the `Nav.vue` file.
* Note that on **Line 16**, we are including the Flags Override view, `rolloutOverride`, made available when the DEV link is seen, gated by `v-if="isDev"`.
* The Flags Override view relies on including the Rollout library for this file as seen on **Line 45**. The `rolloutOverride` function is implemented using the library's `Rox.showOverrides()`, seen on **Line 53**.
* To restrict the Flags Override view and functionality from unintended audiences, we have chosen to show this tool **only on the Development branch deployments**. This is accomplished on **Line 15**, where `isDev` gates the component using the boolean value returned from the function defined on **Line 58**.

### Lab 3 Completed!
Congratulations! You have finished Lab 3 of the CloudBees Rollout Workshop.

**For instructor led workshops please return to the [workshop slides](https://cloudbees-days.github.io/core-rollout-flow-workshop/rollout/#23)**

Otherwise, you may proceed to the next lab: [**User Targeting**](../rolloutTargeting/rolloutTargeting.md) or choose another lab on the [main page](../../README.md#workshop-labs).
