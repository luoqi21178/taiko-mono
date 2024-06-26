<script lang="ts">
  import { Spinner } from '$components/core/Spinner';
  import { classNames } from '$lib/util/classNames';

  import { ButtonState } from './states';
  import type { ActionButtonType } from './types';

  export let loading = false;

  export let priority: ActionButtonType;
  export let state: ButtonState = ButtonState.DEFAULT;

  export let onPopup = false;

  $: if (loading) {
    state = ButtonState.LOADING;
  } else {
    state = ButtonState.DEFAULT;
  }

  $: disabledColor = onPopup && $$restProps.disabled ? '!bg-dialog-interactive-disabled' : '';

  $: commonClasses = classNames(
    'btn size-[56px] px-[28px] py-[14px] rounded-full flex-1 w-full items-center',
    $$restProps.disabled ? 'cursor-not-allowed' : 'cursor-pointer',
    disabledColor,
    $$props.class,
  );

  $: primaryClasses = classNames('btn-primary text-white border-none');

  $: secondaryClasses = classNames(
    $$restProps.disabled
      ? 'border-none'
      : 'border-primary-brand dark:text-white hover:bg-primary-interactive-hover btn-secondary bg-transparent light:text-black',
  );

  $: priorityToClassMap = {
    primary: primaryClasses,
    secondary: secondaryClasses,
  };

  $: classes = classNames(commonClasses, priorityToClassMap[priority], $$props.class);
</script>

<button {...$$restProps} class={classes} on:click>
  {#if loading}
    <Spinner />
  {/if}

  <slot />
</button>
