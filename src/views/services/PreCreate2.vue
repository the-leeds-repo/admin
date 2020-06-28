<template>
  <gov-width-container>
    <vue-headful :title="`${appName} - Add Service`" />

    <gov-back-link :to="{ name: 'services-index' }">
      Back to services
    </gov-back-link>

    <gov-main-wrapper>
      <gov-grid-row>
        <gov-grid-column width="one-half">
          <gov-heading size="xl">Add a new Service Page</gov-heading>

          <gov-body>
            Mandatory information will include, but not be limited to
          </gov-body>

          <gov-list bullet>
            <li>Name of service</li>
            <li>Location</li>
            <li>Logo</li>
            <li>Website</li>
            <li>Accessibility to the service</li>
            <li>Contact details</li>
            <li>Information on who's the service for</li>
          </gov-list>

          <gov-body>
            At the end of this process, you will be able to click "submit". If
            any of the mandatory fields are left blank, these will flag as
            errors.
          </gov-body>

          <gov-body>
            Mandatory information is needed to complete the process. However, if
            you later find out any other optional information that you think
            important, don't worry: you can edit services after adding them.
          </gov-body>

          <gov-list bullet>
            <li>
              Once you've added a service, this will need to be approved by an
              admin before it is live on the platform
            </li>
            <li>
              You can edit the service afterwards
            </li>
          </gov-list>

          <gov-warning-text>
            This form will take about 25 minutes to complete. We cannot save
            your progress, so please make sure you have enough time.
          </gov-warning-text>

          <gov-button :to="{name: 'services-pre-create-1'}">
            Back
          </gov-button><!--
          -->&nbsp;<!--
          --><gov-button :to="{ name: 'services-create' }">
            Continue
          </gov-button>
        </gov-grid-column>
      </gov-grid-row>
    </gov-main-wrapper>
  </gov-width-container>
</template>
